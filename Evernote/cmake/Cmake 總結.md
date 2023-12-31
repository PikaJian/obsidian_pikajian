# Cmake 總結

# CMake使用总结

总结CMake的常用命令，并介绍有用的CMake资源。

CMake意为cross-platform make，可用于管理c/c++工程。CMake解析配置文件CMakeLists.txt生成Makefile，相比直接用Makefile管理工程，CMake更灵活和简单。

## 简单的例子

一个完整的Demo可参考[这里](https://github.com/mawenbao/protobuf-demo)。

假设当前目录的结构为

./a.cpp
./b.cpp

./include/common.h
./include/defines.h

./other/c.cpp
./other/d.cpp

./lib/libB.a
./lib/libBd.a
./lib/libA.so
./lib/libAd.so
./lib/libB.so
./lib/libBd.so
./lib/libC.so
./lib/libCd.so

使用下面的CMakeLists.txt文件，目标是编译当前目录和./other目录下的所有源文件，并链接./lib目录下的相应库文件到最终的可执行文件./bin/hello(或./bin/hellod)。

cmake\_minimum\_required(VERSION 2.8)project(helloworld)

set(CMAKE\_VERBOSE\_MAKEFILE on)set(CMAKE\_CXX\_COMPILER "g++")set(CMAKE\_CXX\_FLAGS "")set(CMAKE\_CXX\_FLAGS\_DEBUG "-g3 -Wall")set(CMAKE\_CXX\_FLAGS\_RELEASE "-O2 -Wall")set(EXECUTABLE\_OUTPUT\_PATH ${PROJECT\_SOURCE\_DIR}/bin)

aux\_source\_directory(./ SRC\_LIST)aux\_source\_directory(./other OTHER\_SRC\_LIST)list(APPEND SRC\_LIST ${OTHER\_SRC\_LIST})

include\_directories(${PROJECT\_SOURCE\_DIR}/include)link\_directories(${PROJECT\_SOURCE\_DIR}/lib)

if(${MY\_BUILD\_TYPE} MATCHES "debug")
    add\_executable(hellod ${SRC\_LIST})
    target\_link\_libraries(hellod Ad Bd.a Cd.so)else()
    add\_executable(hello ${SRC\_LIST})
    target\_link\_libraries(hello A B.a C.so)endif()

执行命令`cmake -DMY_BUILD_TYPE=debug .`生成Makefile，make之后生成./bin/hellod（调试版本），或执行`cmake .`最后生成./bin/hello。

## 常用的CMake变量

详细内容请参考[CMake Useful Variables](http://www.cmake.org/Wiki/CMake_Useful_Variables)。

* PROJECT\_SOURCE\_DIR 工程的源文件目录，通常是包含CMakeLists.txt（有Project命令的）的目录。

### 自定义变量

可在命令行下向CMake传递自定义变量

cmake -DMY\_VAR=hello .

然后在CMakeLists.txt中使用，注意大小写。

message(${MY\_VAR})

### 构建类型

cmake默认支持多种构建类型(build type)，每种构建类型都有专门的编译参数变量，详情见下表[1](http://blog.atime.me/note/cmake.html#fn:1):

| CMAKE\_BUILD\_TYPE | 对应的c编译选项变量 | 对应的c++编译选项变量 |
| --- | --- | --- |
| None | CMAKE\_C\_FLAGS | CMAKE\_CXX\_FLAGS |
| Debug | CMAKE\_C\_FLAGS\_DEBUG | CMAKE\_CXX\_FLAGS\_DEBUG |
| Release | CMAKE\_C\_FLAGS\_RELEASE | CMAKE\_CXX\_FLAGS\_RELEASE |
| RelWithDebInfo | CMAKE\_C\_FLAGS\_RELWITHDEBINFO | CMAKE\_CXX\_FLAGS\_RELWITHDEBINFO |
| MinSizeRel | CMAKE\_C\_FLAGS\_MINSIZEREL | CMAKE\_CXX\_FLAGS\_MINSIZEREL |

在CMakeLists.txt中可以自定义编译选项变量

set(CMAKE\_CXX\_FLAGS\_RELEASE "-Wall -O2")

然后运行cmake的时候，传入相应的构建类型即可

cmake -DCMAKE\_BUILD\_TYPE=Release

## 常用命令

详情可参考对应版本的[CMake文档](http://www.cmake.org/cmake/help/documentation.html)。

### 检查CMake版本

cmake版本至少为2.8

cmake\_minimum\_required(VERSION 2.8)

### 定义工程名称

工程名为helloworld

project(helloworld)

### 查找源文件

查找当前目录下所有的源文件并保存到SRC\_LIST变量里

aux\_source\_directory(. SRC\_LIST)

查找src目录下所有以cmake开头的文件并保存到CMAKE\_FILES变量里

file(GLOB CMAKE\_FILES "src/cmake\*")

`file`命令同时支持目录递归查找

file(GLOB\_RECURSE CMAKE\_FILES "src/cmake\*")

按照官方文档的说法，**不建议**使用file的GLOB指令来收集工程的源文件，原文解释如下

> We do not recommend using GLOB to collect a list of source files from your source tree. If no CMakeLists.txt file changes when a source is added or removed then the generated build system cannot know when to ask CMake to regenerate.

大意就是，GLOB收集的源文件增加或删除，而CMakeLists.txt没有发生修改时，CMake不能识别这些文件。其实，当CMakeLists.txt使用aux\_source\_directory和file glob查找工程源文件时，如果添加或删除源文件，都需要重新运行CMake。

### set命令

经常配合set命令使用的CMake变量，使用`set(variable value)`进行设置。

* `CMAKE_VERBOSE_MAKEFILE` on 输出详细的编译和链接信息
* `CMAKE_CXX_COMPILER` "g++" c++编译器
* `CMAKE_CXX_FLAGS` "-Wall" c++编译器参数
	* `CMAKE_CXX_FLAGS_DEBUG` debug版本对应的编译器参数
	* `CMAKE_CXX_FLAGS_RELEASE` release版本对应的编译器参数
* `EXECUTABLE_OUTPUT_PATH` ${PROJECT\_SOURCE\_DIR}/bin 可执行文件的输出目录
* `LIBRARY_OUTPUT_PATH` ${PROJECT\_SOURCE\_DIR}/lib 链接库的输出目录

set命令还可以设置自定义变量，比如

set(MY\_GREETINGS "hello world")

### 包含目录和链接目录

将`./include`和`./abc`加入包含目录列表

include\_directories(
    ./include
    ./abc
)

将`./lib`加入编译器链接阶段的搜索目录列表

link\_directories(
    ./lib
)

### 添加生成目标

使用SRC\_LIST源文件列表里的文件生成一个可执行文件hello

add\_executable(hello ${SRC\_LIST})

使用SRC\_LIST源文件列表里的文件生成一个静态链接库libhello.a

add\_library(hello STATIC ${SRC\_LIST})

使用SRC\_LIST源文件列表里的文件生成一个动态链接库libhello.so

add\_library(hello SHARED ${SRC\_LIST})

将若干库文件链接到生成的目标hello(libhello.a或libhello.so)

target\_link\_libraries(hello A B.a C.so)

需要注意的是，target\_link\_libraries里库文件的顺序符合gcc链接顺序的规则，即被依赖的库放在依赖它的库的后面，比如上面的命令里，libA.so可能依赖于libB.a和libC.so，如果顺序有错，链接时会报错。还有一点，B.a会告诉CMake优先使用静态链接库libB.a，C.so会告诉CMake优先使用动态链接库libC.so，也可直接使用库文件的相对路径或绝对路径。

### 自定义链接选项

有时候需要自定义链接选项，比如需要单独对B.a使用`--whole-archive`选项，可以

target\_link\_libraryies(hello A -Wl,--whole-archive B.a -Wl,--no-whole-archive C.so)

### 自定义Makefile目标

运行下面的whatever目标`make whatever`，会先创建一个目录`./hello`，然后将当前目录的`a.txt`拷贝到新建的`./hello`目录里。

add\_custom\_command(
    OUTPUT ./hello/a.txt
    COMMAND mkdir -p ./hello 
            cp a.txt ./hello
    DEPENDS a.txt
)
add\_custom\_target(whatever DEPENDS ./hello/a.txt)

自定义目标还可以使用`add_dependencies`命令加入到其他目标的依赖列表里，当执行`make demo`时，whatever目标会被自动调用。

add\_executable(demo ${SRC\_LIST})
add\_dependencies(demo whatever)

### 其他常用命令

包含其他目录的CMakeLists.txt

include(/path/to/another/CMakeLists.txt)

if命令

if(${MY\_BUILD\_TYPE} MATCHES "debug")
    ...
else()
    ...
endif()

list命令

list(APPEND SRC\_LIST 
    a.cpp
    b.cpp
)

list(REMOVE\_ITEM SRC\_LIST
    a.cpp
)

## 更多的例子

### 自定义Makefile目标的完整例子

下面的CMakeLists.txt添加一个自定义目标proto，该目标在编译工程前，会先调用protobuf程序编译先生成Google Protocol Buffers的消息解析器。

cmake\_minimum\_required(VERSION 2.6)project(protobuf-demo)

\# compile proto filesset(PROTO\_IN  news.proto)set(PROTO\_SRC news.pb.cc)set(PROTO\_OUT news.pb.h news.pb.cc proto/)

add\_custom\_command(
    OUTPUT ${PROTO\_OUT}
    COMMAND protoc \--cpp\_out . \--java\_out . ${PROTO\_IN}
    DEPENDS ${PROTO\_IN})add\_custom\_target(proto DEPENDS ${PROTO\_OUT})

aux\_source\_directory(. SRC\_LIST)list(APPEND SRC\_LIST
    ${PROTO\_SRC})

set(CMAKE\_CXX\_COMPILER "g++")set(CMAKE\_CXX\_FLAGS "-Wall")set(CMAKE\_VERBOSE\_MAKEFILE on)

add\_executable(demo ${SRC\_LIST})add\_dependencies(demo proto)target\_link\_libraries(demo protobuf)

## 阅读资料

1. [CMake文档列表](http://www.cmake.org/cmake/help/documentation.html)
2. [CMake常用变量列表](http://www.cmake.org/Wiki/CMake_Useful_Variables)
3. [CMake入门教程](http://www.cmake.org/cmake/help/cmake_tutorial.html)

1. [CMake Useful Variables: Compilers and Tools](http://www.cmake.org/Wiki/CMake_Useful_Variables#Compilers_and_Tools)，引用于2014-05-06。
