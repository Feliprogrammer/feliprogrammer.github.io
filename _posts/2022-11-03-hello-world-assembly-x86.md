{% gist cd16eb3f13c6ab4e4a75d30ec7eedb8d %}

section .data
     msg db "Hello World"
     len equ $-msg

section .text
    global _start:

        mov eax, 4
        mov ebx, 1
        mov ecx, msg
        mov edx, len
        int 0x80 

        mov eax, 1
        mov ebx, 0 
        int 0x80

