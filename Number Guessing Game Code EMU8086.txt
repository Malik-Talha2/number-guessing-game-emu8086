.model small
.stack 100h
.data

msg1 db "Guess Number (0-9): $"
msg2 db 13,10,"Correct!$"
msg3 db 13,10,"Too High!$"
msg4 db 13,10,"Too Low!$"
msg5 db 13,10,"Game Over!$"
msg6 db 13,10,"Score = $"
msg7 db 13,10,"Play Again? (Y/N): $"

random db ?
attempt db ?
score db 0

.code
main proc

mov ax,@data
mov ds,ax

start_new_game:

mov attempt,5

mov ah,00h
int 1Ah

mov al,dl
and al,0Fh

mov bl,10
div bl

mov random,ah

start_game:

cmp attempt,0
je game_over

mov dx,offset msg1
mov ah,09h
int 21h

mov ah,01h
int 21h

sub al,30h

cmp al,random
je correct

jg high

jl low

high:

mov dx,offset msg3
mov ah,09h
int 21h

dec attempt
jmp start_game

low:

mov dx,offset msg4
mov ah,09h
int 21h

dec attempt
jmp start_game

correct:

inc score

mov dx,offset msg2
mov ah,09h
int 21h

mov dx,offset msg6
mov ah,09h
int 21h

mov al,score
add al,30h

mov dl,al
mov ah,02h
int 21h

jmp play_again

game_over:

mov dx,offset msg5
mov ah,09h
int 21h

mov dx,offset msg6
mov ah,09h
int 21h

mov al,score
add al,30h

mov dl,al
mov ah,02h
int 21h

play_again:

mov dx,offset msg7
mov ah,09h
int 21h

mov ah,01h
int 21h

cmp al,'Y'
je start_new_game

cmp al,'y'
je start_new_game

mov ah,4ch
int 21h

main endp
end main