act_ui.app:     file format elf32-little

act_ui.app

architecture: UNKNOWN!, flags 0x00000112:

EXEC_P, HAS_SYMS, D_PAGED

start address 0x000ea745

  

Program Header:

0x70000001 off    0x00625ae4 vaddr 0x00635ae4 paddr 0x00635ae4 align 2**2

         filesz 0x00007980 memsz 0x00007980 flags r--

    PHDR off    0x00000034 vaddr 0x00010034 paddr 0x00010034 align 2**2

         filesz 0x00000100 memsz 0x00000100 flags r-x

  INTERP off    0x00000134 vaddr 0x00010134 paddr 0x00010134 align 2**0

         filesz 0x00000019 memsz 0x00000019 flags r--

    LOAD off    0x00000000 vaddr 0x00010000 paddr 0x00010000 align 2**16

         filesz 0x0062d468 memsz 0x0062d468 flags r-x

    LOAD off    0x0062d468 vaddr 0x0064d468 paddr 0x0064d468 align 2**16

         filesz 0x00029d54 memsz 0x005f4378 flags rw-

 DYNAMIC off    0x00635020 vaddr 0x00655020 paddr 0x00655020 align 2**2

         filesz 0x000002b0 memsz 0x000002b0 flags rw-

    NOTE off    0x00000150 vaddr 0x00010150 paddr 0x00010150 align 2**2

         filesz 0x00000044 memsz 0x00000044 flags r--

   STACK off    0x00000000 vaddr 0x00000000 paddr 0x00000000 align 2**4

         filesz 0x00000000 memsz 0x00000000 flags rwx


這是使用 readelf -l act_ui.app 或類似指令得到的 **Program Header Table**（也叫「段表」），是 ELF 格式在執行時用來告訴 Loader（例如 Linux kernel 或 ld.so）如何把檔案內容對應到記憶體中。

---

## **✅ TL;DR：這份輸出是「實際執行時記憶體分布」的 blueprint，包含：**

- 哪些區段該載入
    
- 載到哪個虛擬位址（vaddr）
    
- 權限（可讀/可寫/可執行）
    
- 對應檔案內的 offset 和大小
    

---

## **🔍 重點欄位說明（你關注的應該是 .text 實際載入位置）**

### **✅.text 等 code segment 對應這一段：**
```
LOAD off    0x00000000 vaddr 0x00010000 paddr 0x00010000 align 2**16
     filesz 0x0062d468 memsz 0x0062d468 flags r-x
```

|**欄位**|**說明**|
|---|---|
|LOAD|這是一個可執行的 segment（應該包含 .text, .plt, .rodata, 甚至 .init, .fini）|
|off=0x0|從檔案的最開頭讀入|
|vaddr=0x00010000|**實際執行時會把這段載入到虛擬記憶體 0x10000 開始的位址**|
|filesz=0x0062d468|檔案中這段內容長度（大約 6MB）|
|flags=r-x|read + execute（不能寫） → 就是 **code section**|

### **✅ .data**, .bss 等資料區：**
這一段是：

- offset 對應 .data 開始
    
- vaddr = 0x0064d468 是實際資料載入位址
    
- memsz > filesz → 表示含有 .bss（未初始化區段）
    
- 權限是 rw-：代表這段可寫（但不能執行）

## **✅ 結論整理**

|**問題**|**結論**|
|---|---|
|.text 實際執行地址在哪？|0x00010000 開始（VMA），從這邊進入 code|
|是否有 ASLR / PIE / relocate？|否。這是 EXEC binary，不會 relocate|
|.data / .bss 等寫入資料地址？|0x0064d468 開始（根據第二段 LOAD）|
|怎麼對應 .text 段？|.text → 包在第一個 r-x 的 LOAD segment|

你如果要在 GDB 中設 breakpoint 或分析 crash address，就會根據這些 VMA 來對應 ELF 的 section。

---

如果你還想反查「某個 address 是哪個 function」，可以用：

```
addr2line -e act_ui.app.debug 0x000ea745
```
