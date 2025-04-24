### **dma_pool_alloc() 的來源**

- 是用於小型固定大小（例如 64B、256B、4KB）的 **coherent DMA buffer 分配**。
    
- 它是建立在 dma_alloc_coherent() 基礎上。
    
- 其最終會呼叫 dma_alloc_attrs() → dma_alloc_from_contiguous()（若使用 CMA）或 fall back 到 alloc_pages()（從 buddy 系統分配）。

| **組件**               | **來源區域**                                                                      |
| -------------------- | ----------------------------------------------------------------------------- |
| dma_pool_alloc()     | 預設會從 kernel 的 **ZONE_DMA / DMA32** 分配，透過 dma_alloc_coherent() 或 alloc_pages()|
| dma_alloc_coherent() | 若平台支援並啟用 CMA → **會走 CMA**（如 ARM、ARM64）                                        |
| ION                  | 完全獨立於上述，需透過 ion allocator 明確使用                                                |

### **❗重點是： dma_pool_alloc() 不會自動用 ION，也不會總是用 CMA**

  

除非你 platform 的 dma_ops 被設計成使用 CMA，或你 device 被指定進入 CMA domain。
這在 ARM/ARM64 中比較常見：

- arch/arm/mm/dma-mapping.c 或 arm64/mm/dma-mapping.c 會透過 dma_contiguous_reserve() 把一塊記憶體保留給 dma_alloc_coherent() 用（→ 這塊就是 CMA）
- 
### **如果你希望改用 ION 分配並做 DMA（例如做大 buffer、camera buffer）**

```c
void *vaddr = ion_alloc_xxx(...);
dma_addr_t dma = dma_map_single(dev, vaddr, size, DMA_TO_DEVICE);
```

### **CMA大小可在bootarg 寫下 cma=1M@0x2100000-0x2200000**