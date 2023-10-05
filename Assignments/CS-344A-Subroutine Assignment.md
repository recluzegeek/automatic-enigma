---
title: CS-344A-Subroutine Assignment
updated: 2022-09-20 14:06:09Z
created: 2022-09-19 13:02:06Z
latitude: 50.11092210
longitude: 8.68212670
altitude: 0.0000
---


<h1>
<br>
<center>Assignment # 02</center>
 Muhammad Salman <br>212370003<br> CS-344 Computer Organization and Assembly Language
<br>

</h1>

# 1. Recursive function to calculate the Fibonacci of a number

```c++
[org 0x0100]
start:	
mov ax, 4
sub sp,2
push ax
call fibonacci
pop ax
terminate:			
mov ax, 0x4c00
0x21

fibonacci:
 push bp 
 mov bp,sp 
 sub sp,2 ;creating local variable 
 mov bx, [bp+4] ; get passed argument 
 cmp bx,0 ; if num <=0 
 jle return0 ; fibonacci=0 
 cmp bx,1 ; if num == 01 in binary
 je return1 ; fibonacci=1 


 ; else 
 dec bx 
 push bx ;cleared by ret 2 

 call fibonacci 
 
 mov [bp-2],ax ;save ax, in local var 
 mov bx, [bp+4] ; get passed argument 
 sub bx,2 
 push bx ;cleared by ret 2 
 call fibonacci 
 mov dx,[bp-2] ;get previous val of ax, saved above 
 add dx,ax 
 mov ax,dx ; return fibonacci in ax 
 jmp retfromfunc 
return0: 
 mov ax,0 
 jmp retfromfunc 
return1: 
 mov ax,1 
retfromfunc: 
 mov sp,bp ;restore sp-2, which was done for local variable 
 pop bp 
 ret 2 ;clear the stack (discard bx, pushed before calling this subroutine)
```