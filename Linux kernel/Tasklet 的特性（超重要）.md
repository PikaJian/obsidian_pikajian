
tasklet 仍然是 **softirq context**：

- 不能 sleep
    
- 不能用 mutex
    
- 允許 spinlock（但是要用 `spin_lock_bh()`/`spin_lock_irqsave()` 視情況）
    
- 同一個 tasklet instance 不會在同 CPU 同時 re-enter  
    **但可以在不同 CPU 同時跑**（取決於你怎麼 schedule，和 kernel config）