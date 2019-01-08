# ANSI-Project-3
Generates a specific image based on the number inputted



%include "asm_io.inc"


SECTION .data

        error_message: db "incorrect command line argument",10,0
        error2_message: db "Argument must be less than or equal to 9 and greater than or equal to 2",10,0
        num1: db "        o|o",10,0
        num2: db "       oo|oo",10,0
        num3: db "      ooo|ooo",10,0
        num4: db "     oooo|oooo",10,0
        num5: db "    ooooo|ooooo",10,0
        num6: db "   oooooo|oooooo",10,0
        num7: db "  ooooooo|ooooooo",10,0
        num8: db " oooooooo|oooooooo",10,0
        num9: db "ooooooooo|ooooooooo",10,0
        exes: db "xxxxxxxxxxxxxxxxxxxxxxx",10,0
        one: db "Initial configuration",10,0
        fine: db "Final configuration",10,0
SECTION .bss
        peg: resd 9
        discs: resd 1

SECTION .text

global asm_main
sorthem:
  enter 0,0
  pusha


  mov ebx, [ebp +8]
  mov ecx, dword [ebp + 12]



  cmp ecx, 1
  je s_end
  ;mov eax, 0
  mov eax, ecx
  dec eax
  push eax
  mov eax, ebx
  add eax, 4
  push eax
  call sorthem
  add esp, 8


mov edx, 0
  mov esi,0
  Loop:
   cmp edx, ecx
   je Loop_end
   mov eax, [ebx + esi]
   mov edi, [ebx + esi + 4]
   cmp eax, edi
   jg Loop_end
   mov [ebx + esi + 4], eax
   mov [ebx + esi ], edi
   inc edx
   add esi, 4
   jmp Loop
  Loop_end:
   mov eax, 0
   push dword [discs]
   push peg
   call showp
   add esp,8

 s_end:
  popa
  leave
  ret

showp:
        enter 0,0   ;format
        pusha


        mov ebx, [ebp + 8]    ; move array from stack into variable
        mov eax, [ebp + 12]   ; number of disks
        dec eax               ; -1
        mov esi, 4            ; allah said
        mul  esi                 ; cuz y not
        mov ecx, eax          ; move variable to diff


        LOOP:

          mov eax, 0              ; clear the skin

          mov eax, [ebx + ecx]    ; element we are on while iteraing backwards

          cmp ecx, 0           ; check if iterations =9, if it is jmp end
          jl enduh

          sub ecx, 4           ; iterate one down
cmp eax, 0            ; check if value is 0, if it is jmp end
          je  enduh


          cmp eax, 1            ; if 1
          je case1

          cmp eax, 2            ; if 2
          je case2

          cmp eax, 3            ; if 3
          je case3

          cmp eax, 4            ; if 4
          je case4

          cmp eax, 5            ; if 5
          je case5

          cmp eax, 6            ; if 6
          je case6

          cmp eax, 7            ; if 7
          je case7

          cmp eax, 8            ; if 8
          je case8

          cmp eax, 9            ; if 9
          je case9

          case1:
            mov eax, num1
            call print_string
            jmp LOOP

          case2:
            mov eax, num2
            call print_string
            jmp LOOP

          case3:
            mov eax, num3
            call print_string
            jmp LOOP

          case4:
            mov eax, num4
            call print_string
           jmp LOOP

case5:
            mov eax, num5
            call print_string
            jmp LOOP

          case6:
            mov eax, num6
            call print_string
            jmp LOOP

          case7:
            mov eax, num7
            call print_string
            jmp LOOP

          case8:
            mov eax, num8
            call print_string
            jmp LOOP

          case9:
            mov eax, num9
            call print_string
            jmp LOOP

      enduh:
          mov eax,exes
          call print_string
          popa
          leave
          ret

asm_main:

        enter 0,0 ;format
        pusha

        mov eax, dword [ebp+8] ; number of args
        cmp eax, dword 2        ; compare 2
        jne error               ; not 2 bad
        ; DONE CHECKING NUMBER OF ARGUMENTS

        mov ebx, dword [ebp+12]  ; address of argv[]
        mov eax, dword [ebx+4]   ; argv[1]
        mov ecx, eax
        mov bl, byte [eax + 1]

cmp bl, 0
        jne error
        ;DONE CHECKING LENGTH OF COMMAND LINE ARGUMENT

        mov bl,byte[eax]   ; moves value
        mov eax,0          ; clear eax cuz BAD
        mov al, bl        ; no reason DOG
        sub al , '0'       ; from character to number

        cmp eax, dword 2  ; less than 2 bad
        jl error2
        cmp eax, dword 9  ; more than 9 bad
        jg error2

        mov ecx, peg
        mov [discs], eax
        push eax                ; number of disks
        push ecx                ; address peg

        mov ebx, ecx            ; rconf bad

        call rconf             ; make array using rconf
        mov eax, one
        call print_string
        call showp             ; lets find out

        call sorthem
        mov eax, fine
        call print_string
        call showp
        add esp, 8
        jmp end

error:
        mov eax, error_message
        call print_string
        jmp end


error2:
       mov eax, error2_message
       call print_string
       jmp end

end:
        popa
        mov eax, 0
        leave
        ret

