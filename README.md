.model small
.stack 100h

.data
    msg1 db "Enter first digit (0-9): $"
    msg2 db "Enter operator (+ - * /): $"
    msg3 db "Enter second digit (0-9): $"
    resultMsg db "Result = $" 
    errorMsg db "Error: Division by zero!$"
    newline db 13,10,"$"

.code

main proc
    mov ax, @data
    mov ds, ax

start_calc:

    ; ---- print msg1 ----
    mov dx, offset msg1
    mov ah, 9
    int 21h
    ; first number
    mov ah, 1
    int 21h
    sub al, 30h
    mov bl, al      

    call newlinePrint

    ; ---- print msg2 ----
    mov dx, offset msg2
    mov ah, 9
    int 21h
    ; operator
    mov ah, 1
    int 21h
    mov bh, al      

    call newlinePrint

    ; ---- print msg3 ----
    mov dx, offset msg3
    mov ah, 9
    int 21h
    ; second number
    mov ah, 1
    int 21h
    sub al, 30h
    mov cl, al      

    call newlinePrint

    ; ---- Decide operation ----
    mov al, bl

    cmp bh, '+'
    je addOp

    cmp bh, '-'
    je subOp

    cmp bh, '*'
    je mulOp

    cmp bh, '/'
    je divOp

    jmp start_calc 

addOp:
    add al, cl
    jmp print_result
    
subOp:
    sub al, cl
    jmp print_result

mulOp:
    mov al, bl
    mul cl          
    jmp print_result

divOp:
    cmp cl, 0
    je zero_error

    mov ah, 0
    mov bl, cl
    div bl          ; AL = AL / BL
    jmp print_result

; ---- Division by Zero ----
zero_error:
    mov dx, offset errorMsg
    mov ah, 9
    int 21h
    call newlinePrint
    jmp start_calc

; ---- Print Result ----
print_result:
    add al, 30h     

    mov dx, offset resultMsg
    mov ah, 9
    int 21h

    mov dl, al
    mov ah, 2
    int 21h

    call newlinePrint
    jmp start_calc

main endp

; ---- New line function ----
newlinePrint proc
    mov dx, offset newline
    mov ah, 9
    int 21h
    ret
newlinePrint endp

end main
