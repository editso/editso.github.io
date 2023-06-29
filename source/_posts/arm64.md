---
title: Arm64汇编记录
date: 2023-6-29 10:37
tags:
    - ASM
    - Arm
    - Arm64
categories:
  - 汇编学习
---

# 相关指令记录

## ARDP
> ADRP \<reg> \<imm>  

1. 从PC相对地址到4KB页添加一个直接值
2. 将当前`pc`地址`低12位置零`, 并加上`立即数左移12位后`的值
    
例子:
```
PC        = 0xe7b27341c7b0
reg = x16
imm = 866

; adrp x16, 866

x16       = (0xe7b27341c7b0 & (~0xFFF)) + (866 << 12)
          = 0xe7b27377e000
```

## CBZ
> CBZ \<reg> \<imm>

1. 将寄存器的值与`0`比较, 如过`相等则跳转`到相对`pc` `+/-1MB`的位置

例子:
```
reg =  x0
imm =  0xe7b273333c84

; cbz x0, 0xe7b273333c84

if (x0 == 0 ) goto 0xe7b273333c84
...continue
```

## XZR
用于64位(8字节)置零操做

例子:
```
; 将x20寄存器置零
; mov x20, xzr

x20 = 0
```

## LDP
> LDP \<reg1>, \<reg2>, [reg3], \<imm>  

  1. 将`reg3`地址里面的值加载到`reg1`和`reg2`
  2. 然后更新`reg3`的值, `reg3 += imm`

例子:
```
sp = uint64[]{ 2, 3 }

; ldp, x0, x1, [sp], #0x10

x0 =  *sp
   = 2
x1 = *(sp + 8)
   = 3
sp = sp + 16
```

## STP
> STP \<reg1>, \<reg2>, [reg3, imm]!

1. 将`reg1`,`reg2`中的值存入 `reg3 + imm`的地址里面, 并更新 `reg3`的值 `reg3 += imm`

例子:
```
x1 = 1
x2 = 2

; stp x1, x2, [sp, #-10]!

*(sp - 16)       = x1
                 = 1
*(sp - 16 - 8)   = x2
                 = 2
sp               = sp - 16
```