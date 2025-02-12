# 汇编语言实验

#### 实验四、`[bx]`和`loop`的使用

1. 编程，向内存`0:200~0:23F`依次传送数据`0~63(3FH)`

```assembly
assume cs:code
code segment
	mov ax, 0020h
	mov ds, ax
	mov bx, 0
	mov cx, 64
s:  mov ds:[bx], bl
	inc bx
	loop s
	mov ax, 4c00h
	int 21h
code ends
end
```

- 复制不需要考虑数据溢出，最大值为63，故`bx`与`bl`的值是一致的
- 内存地址与传送的数据有一致性，故去偏移地址`0~3F`，此时段地址为`0020`

2. 补全程序，将"`mov ax, 4c00h`"之前的指令复制到内存`0:200`处，上机调试，跟踪运行结果

```assembly
assume cs:code
code segment
	mov ax, cs
	mov ds, ax
	mov ax, 0020h
	mov es, ax
	mov bx, 0
	mov cx, 23
s:  mov al, [bx]
	mov es:[bx], al
	inc bx
	loop s
	mov ax, 4c00h
	int 21h
code ends
end
```

（1）复制的是什么？从哪里到哪里？

答：复制的是`CS:IP`所指向的命令的字节内容，从`CS:IP~CS:IP+23`到指定的`0020:IP~0020:IP+23`

（2）复制的有多少字节？

答：总共23个字节，`debug`中使用`u`命令查看`cs:0`

#### 实验五、编写、调试具有多个段的程序