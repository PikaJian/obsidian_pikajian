# send syscall

This is a brief introduction about how Linux kernel handles the `send` system call.

This study is based on kernel version 3.7.2, which is the latest stable kernel when writing this study.

## <https://gist.github.com/yegle/4560621#how-system-call-is-defined>How system call is defined

In the latest kernel, the system call is defined using the `SYSCALL_DEFINEx` macro, in which `x` is the number of arguments. For example, in order to find the definition of `asmlinkage long sys_sendto(int, void __user *, size_t, unsigned, struct sockaddr __user *, int);`, you need to `grep` for`SYSCALL_DEFINE6` because it has 6 arguments.

The definition of the system call `send` can be found at `net/socket.c`.

## <https://gist.github.com/yegle/4560621#how-is-the-send-system-call-handled>How is the `send` system call handled

### <https://gist.github.com/yegle/4560621#definition-of-send-system-call>Definition of `send` system call

Here's the code of the `send` system call

    /*
     *  Send a datagram down a socket.
     */
    SYSCALL_DEFINE4(send, int, fd, void __user *, buff, size_t, len,
            unsigned int, flags)
    {
        return sys_sendto(fd, buff, len, flags, NULL, 0);
    }
    

From the code we know that the `send` system call is just a simple wrapper function around another system call `sendto`.

### <https://gist.github.com/yegle/4560621#definition-of-sendto-system-call>Definition of `sendto` system call

The definition of `sendto` system call is available in the same file, `net/socket.c`.

    /*
     *  Send a datagram to a given address. We move the address into kernel
     *  space and check the user space data area is readable before invoking
     *  the protocol.
     */
    SYSCALL_DEFINE6(sendto, int, fd, void __user *, buff, size_t, len,
            unsigned int, flags, struct sockaddr __user *, addr,
            int, addr_len)
    

Here's the definition of each argument from `man 2 sendto`:

* `int fd`: the file descriptor of the socket, which is used to send the data to
* `void __user * buff`: the content which will be send by this system call
* `size_t len`: the length of the `buff`
* `unsigned int flags`: the bitwise OR of some predefined constants
* `struct sockaddr __user * addr`: the destination address. The `send` system call will set it to NULL
* `int addr_len`: the length of the `addr` struct, which `send` system call will set it to zero

Here's the description about `sendto`'s behaviour:

1. use `sockfd_lookup_light` to find the corresponding socket descriptor and return it
	
	This function will try to valid if the provided integer `fd` is a valid file descriptor, then return the sockeet if it's valid, otherwise return `NULL`.
	
2. The message to be send will be assembled, then `sock_sendmsg` will be called to send the message, whose return value will be returned by `sendto` then returned to `send` callee.
	

### <https://gist.github.com/yegle/4560621#definition-of-sock_sendmsg-call>Definition of `sock_sendmsg` call

Below is the description of the `sock_sendmsg` function call

1. call `init_sync_kiocb`, which just inited a new `kiocb` struct. `kiocb` is used as callback if the current system call is asynchronous.
2. call `__sock_sendmsg`, which is a wrapper around `security_socket_sendmsg` and`__sock_sendmsg_nosec`.
	
	The kernel will try to call `security_socket_sendmsg` to check the permission before transmitting, and then call `__sock_sendmsg_nosec` to send it.
	
	Function `__sock_sendmsg_nosec` will call `sock->ops->sendmsg` to send the message
	
	The `socket->ops` is a `proto_ops`, which is a protocol-specific struct.
	

### <https://gist.github.com/yegle/4560621#definition-of-proto_ops-struct-and-its-sendmsg-function>Definition of `proto_ops` struct and its `sendmsg` function

This is a protocol-specific. Depending on the socket type (IPv6/IPv4, UDP/TCP/RAW), there's different implementation of this struct.

For most socket types (at least for IPv4+TCP, IPv4+UDP, IPv4+RAW), the `inet_sendmsg` function will be called to sendmsg.

In function `inet_sendmsg`, if the current socket is not `bind`ed, kernel will call `inet_autobind` to bind the socket to a local IP/port.

In `inet_sendmsg`, it will call `socket->sk_prot->sendmsg`. We finally reach the transport layer -> internet layer interface.

## <https://gist.github.com/yegle/4560621#internet-layer-functions>Internet Layer functions
