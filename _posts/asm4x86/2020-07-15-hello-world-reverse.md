---
layout: single
title:  "6.10.1 简答题"
tags: asm
categories: asm4x86
---
番外: Hello, world! 的反汇编

## Windows VS2019

### C++源码
```cpp
#include <cstdio>

int main()
{
    int a = 0x12345678;
    short b = 0x12345678;

    printf("a: %x\nb: %x", a, b);

    return 0;
}
```

### VS调试模式下反汇编代码
```asm
--- C:\Users\admin\work\helloworld\helloworld\helloworld.cpp -------------------
#include <cstdio>

int main()
{
008D1810  push        ebp  
008D1811  mov         ebp,esp  
008D1813  sub         esp,0D8h  
008D1819  push        ebx  
008D181A  push        esi  
008D181B  push        edi  
008D181C  lea         edi,[ebp-0D8h]  
008D1822  mov         ecx,36h  
008D1827  mov         eax,0CCCCCCCCh  
008D182C  rep stos    dword ptr es:[edi]  
008D182E  mov         ecx,offset _4A53B540_helloworld@cpp (08DC003h)  
008D1833  call        @__CheckForDebuggerJustMyCode@4 (08D1217h)  
    int a = 0x12345678;
008D1838  mov         dword ptr [a],12345678h  
    short b = 0x12345678;
008D183F  mov         eax,5678h  
008D1844  mov         word ptr [b],ax  

    printf("a: %x\nb: %x", a, b);
008D1848  movsx       eax,word ptr [b]  
008D184C  push        eax  
008D184D  mov         ecx,dword ptr [a]  
008D1850  push        ecx  
008D1851  push        offset string "a: %x\nb: %x" (08D7B30h)  
008D1856  call        _printf (08D1046h)  
008D185B  add         esp,0Ch  

    return 0;
008D185E  xor         eax,eax  
}
008D1860  pop         edi  
008D1861  pop         esi  
008D1862  pop         ebx  
008D1863  add         esp,0D8h  
008D1869  cmp         ebp,esp  
008D186B  call        __RTC_CheckEsp (08D1221h)  
008D1870  mov         esp,ebp  
008D1872  pop         ebp  
008D1873  ret  
--- 无源文件 -----------------------------------------------------------------------
```

### 说明

