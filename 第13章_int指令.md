### int指令
```
格式: int n, n为中断类型码, 功能是引发中断过程

(1) 取中断类型码
(2) 标志寄存器入栈, TF = 0, IF = 0
(3) CS、IP入栈
(4) IP = n * 4, CS = n * 4 + 2
```

### BIOS和DOS所提供的中断例程
```
在系统板的ROM中存放着一套程序, 称为BIOS(基本输入输出系统), 主要包含以下几部分内存:
    (1) 硬件系统的检测和初始化程序
    (2) 外部中断和内部中断的中断例程
    (3) 用于对硬件设备进行I/O操作的中断例程
    (4) 其他和硬件系统相关的中断例程

可以用int指令直接调用BIOS和DOS提供的中断例程来完成某些工作
```
