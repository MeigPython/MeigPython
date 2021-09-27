gc 模块实现内存垃圾回收机制，该模块实现了CPython模块相应模块的子集。更多信息请参阅CPython文档：gc
### 启用自动回收内存碎片机制
>gc.enable()

启用自动回收内存碎片机制。

### 禁用自动回收机制
>gc.disable()

禁用自动回收机制。
### 回收内存碎片
>gc.collect()

回收内存碎片。
### 返回分配的堆RAM的字节数
>gc.mem_alloc()

返回分配的堆RAM的字节数。此功能是MicroPython扩展。
### 返回可用堆RAM的字节数
>gc.,e,_free()

返回可用堆RAM的字节数，如果此数量未知，则返回-1.此功能是Micopython扩展。
