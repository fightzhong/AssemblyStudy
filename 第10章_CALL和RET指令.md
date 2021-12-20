### 指令解析
```
ret指令: 用栈中的数据, 修改IP的内容, 从而实现近转移 (弹出两个字节来替换IP的值, 等于一次pop操作, 因为栈是基于字操作的)
retf指令: 用栈中的数据, 修改CS和IP的内容, 从而实现远转移

CPU执行ret指令时:
    (1) ip = ss * 16 + sp
    (2) sp = sp + 2
    -----相当于
        POP IP
    


CPU执行retf指令时:
    (1) ip = ss * 16 + sp
    (2) sp = sp + 2
    (3) cs = ss * 16 + sp
    (4) sp = sp + 2
    -----相当于
        POP IP
        POP CS

call指令: 
    (1) 将当前的IP或CS和IP压入栈中
    (2) 转移
```

### call指令的几种形式
```
形式一: call 标号     [将当前的IP压栈后, 转到标号处执行指令, 这是依据位移来进行转移, 即IP与标号之间的位移, 此时CS是没有发生改变的]
            (1) sp = sp - 2
            (2) ss * 16 + sp = ip
            (3) ip = ip + 16位位移

形式二: call far ptr 标号       [实现段间转移]
            (1) sp = sp - 2
            (2) ss * 16 + sp = cs
            (3) sp = sp - 2
            (4) sp * 16 + sp = ip
            (5) CS = 标号所在段的段地址
            (6) IP = 标号在段中的偏移地址

            -----相当于
                PUSH CS
                PUSH IP
                jmp far prt 标号

形式三: call 16位寄存器 
            (1) SP = SP - 2
            (2) SS * 16 + SP = IP
            (3) IP = 16位寄存器的值
            -----相当于
                 PUSH IP
                 jmp 16位寄存器

形式四: call word ptr 内存单元地址
            -----相当于
                PUSH IP
                jmp word ptr 内存单元地址

       call dword ptr 内存单元地址
            -----相当于
                PUSH CS
                PUSH IP
                jmp dword ptr 内存单元地址
```

### mul指令
```
(1) 两个相乘的数, 要么都是8位, 要么都是16位, 如果是8位, 一个默认放在AL中, 另一个默认放在8位寄存器或者内存单元中, 如果是
    16位, 一个默认放在AX中, 另一个放在16位寄存器或者内存单元中
(2) 结果: 如果是8位乘法, 结果默认在AX中, 如果是16位乘法, 结果高位存在DX中, 低位存在AX中
```
