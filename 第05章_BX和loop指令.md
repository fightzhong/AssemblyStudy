### bx和内存单元的描述
```
[bx]: 表示偏移地址为bx, 在asm代码中, mov ax, [0]会被当成mov ax, 0来看待, 而不是将段地址为ds,
      偏移地址为0的数据放到ax寄存器中, 换句话说, 编译器不会将[常量值]当成偏移地址, 对于这种情况,
      需要用到bx寄存器来代表偏移地址, 即mov ax, [bx], 当然, 如果直接在debug中输入指令mov ax, [0]
      确是可以将偏移地址为0的数据送入到ax寄存器中

汇编代码将内存单元为10H的数据放入到ax寄存器的方式:
    方式一: 
        mov bx, 10H 
        mov ax, [bx]
    方式二:
        mov bx, 10H 
        mov ax, ds:[bx]
    方式三:
        mov ax, ds:[10H]

注意: 通过ds:[bx]是能访问对应内存地址下的内容的, 但是ds:[ax]则不行, 中括号中的值只能是bx寄存器或者常量值, 不能
是其他寄存器的值, 否则会报编译错误
```

### loop指令的作用及执行流程
```
loop指令用于循环执行指令, loop <标号>表示循环执行标号开始的代码, cx中的值表示循环的次数, 比如

assume cs:codes
codes segment
start:
	mov bx, 0
    mov cx, 6h
s:	add ax, bx
	inc bx
	loop s

	mov ax, 4c00h
	int 21h
codes ends 
end start

上面的汇编代码表示循环执行s标号下两行代码6次

loop指令的执行流程:
    <1> cx = cx - 1
    <2> 判断cx是否为0, 如果为0, 则不继续执行, 如果不为0, 则ip跳转到标号对应的代码地址
有点类似于do...while

直接执行循环完毕的两个方案: p命令来替换t命令执行循环命令  或者 g命令跳转到后面的代码地址, g 段地址:偏移地址
```

