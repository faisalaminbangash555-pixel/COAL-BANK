; =====================================================
;      COAL BANK MANAGEMENT SYSTEM - EMU8086
;  MULTI-USER + DYNAMIC ADMIN + TRANSACTION HISTORY
;         + LIVE BALANCE + LIQUIDITY
; =====================================================

.model small
.stack 100h

.data

; =====================================================
; UI DESIGN
; =====================================================

welcomeMsg db 10,13
db "#############################################################",10,13
db "#                                                           #",10,13
db "#               COAL BANK MANAGEMENT SYSTEM                 #",10,13
db "#                                                           #",10,13
db "#############################################################$"

loadingMsg db 10,13
db "Initializing Banking System...",10,13
db "Loading Secure Modules...",10,13
db "Please Wait...$"

mainMenu db 10,13
db "============================================================",10,13
db "                        MAIN MENU                           ",10,13
db "============================================================",10,13
db "[1] Register Account",10,13
db "[2] Login",10,13
db "[3] Exit",10,13
db "[4] Admin Mode",10,13
db "------------------------------------------------------------",10,13
db "Enter Your Choice: $"

userMenu db 10,13
db "============================================================",10,13
db "                       USER DASHBOARD                       ",10,13
db "============================================================",10,13
db "[1] Deposit Money",10,13
db "[2] Withdraw Money",10,13
db "[3] Balance Inquiry",10,13
db "[4] Account Details",10,13
db "[5] View Transaction History",10,13
db "[6] Change PIN",10,13
db "[7] Transfer Money",10,13
db "[8] Logout",10,13
db "------------------------------------------------------------",10,13
db "Enter Your Choice: $"

adminMenuText db 10,13
db "============================================================",10,13
db "                   ADMIN CONTROL PANEL                      ",10,13
db "============================================================",10,13
db "[1] View Master Ledger",10,13
db "[2] View Bank Asset Liquidity",10,13
db "[3] Exit to Main Menu",10,13
db "------------------------------------------------------------",10,13
db "Enter Your Choice: $"

; =====================================================
; INPUT PROMPTS
; =====================================================

nameMsg           db 10,13,"Enter Account Holder Name: $"
accMsg            db 10,13,"Enter Account Number: $"
pinMsg            db 10,13,"Enter 4-Digit PIN: $"
loginAccMsg       db 10,13,"Enter Account Number: $"
loginPinMsg       db 10,13,"Enter PIN: $"
depositMsg        db 10,13,"Enter Deposit Amount: $"
withdrawMsg       db 10,13,"Enter Withdraw Amount: $"
transferAccMsg    db 10,13,"Enter Receiver Account Number: $"
transferMsg       db 10,13,"Enter Transfer Amount: $"
initialDepositMsg db 10,13,"Enter Initial Deposit Amount: $"

; Live balance indicator shown inside transfer before amount entry
senderBalMsg      db 10,13
                  db "------------------------------------------------------------",10,13
                  db "Your Available Balance: $"

adminPromptMsg    db 10,13
db "============================================================",10,13
db "        System First-Time Setup: Please configure          ",10,13
db "              the Master Admin PIN:                        ",10,13
db "============================================================",10,13
db "New Admin PIN: $"

adminLoginMsg     db 10,13,"Enter Master Admin PIN: $"

; =====================================================
; STATUS  RESULT MESSAGES
; =====================================================

successMsg db 10,13
db "============================================================",10,13
db "              TRANSACTION  SUCCESSFUL                       ",10,13
db "============================================================$"

succesMsg db 10,13
db "============================================================",10,13
db "               PIN  CHANGE  SUCCESSFUL                      ",10,13
db "============================================================$"

loginFailMsg db 10,13
db "============================================================",10,13
db "                 INVALID LOGIN DETAILS                      ",10,13
db "============================================================$"

insufficientMsg db 10,13
db "============================================================",10,13
db "                 INSUFFICIENT BALANCE                       ",10,13
db "============================================================$"

blockedMsg db 10,13
db "============================================================",10,13
db "                 ACCOUNT TEMPORARILY BLOCKED                ",10,13
db "============================================================$"

duplicateMsg db 10,13
db "============================================================",10,13
db "               ACCOUNT ALREADY REGISTERED                   ",10,13
db "============================================================$"

systemFullMsg db 10,13
db "============================================================",10,13
db "           SYSTEM FULL - MAX 5 ACCOUNTS REACHED             ",10,13
db "============================================================$"

invalidRecMsg db 10,13
db "============================================================",10,13
db "               INVALID RECEIVER ACCOUNT                     ",10,13
db "============================================================$"

logoutMsg db 10,13
db "============================================================",10,13
db "                    LOGGING OUT...                          ",10,13
db "============================================================$"

noHistoryMsg db 10,13
db "============================================================",10,13
db "              NO TRANSACTION HISTORY FOUND                  ",10,13
db "============================================================$"

; =====================================================
; ACCOUNT DETAIL LABELS
; =====================================================

balanceText   db 10,13,"Current Balance:     $"
holderText    db 10,13,"Account Holder Name: $"
accountText   db 10,13,"Account Number:      $"
availableText db 10,13,"Available Balance:   $"

detailsText db 10,13
db "============================================================",10,13
db "                    ACCOUNT DETAILS                         ",10,13
db "============================================================$"

; =====================================================
; TRANSACTION HISTORY DISPLAY LABELS
; =====================================================

historyHeader db 10,13
db "============================================================",10,13
db "                  TRANSACTION HISTORY                       ",10,13
db "============================================================$"

histTypeDepMsg  db 10,13,"Type: Deposit           | Amount: $"
histTypeWthMsg  db 10,13,"Type: Withdrawal        | Amount: $"
histTypeSndMsg  db 10,13,"Type: Sent Transfer     | Amount: $"
histTypeRcvMsg  db 10,13,"Type: Received Transfer | Amount: $"

histPeerNone    db " | Peer: None$"
histToAcc       db " | To Acc:   $"
histFromAcc     db " | From Acc: $"
histNameLabel   db " | Name: $"

; =====================================================
; ADMIN LEDGER DISPLAY LABELS
; =====================================================

ledgerHeader db 10,13
db "============================================================",10,13
db "          MASTER LEDGER - ALL REGISTERED ACCOUNTS           ",10,13
db "============================================================",10,13
db "$"

ledgerRowPrefix db 10,13,"  Acc No: $"
ledgerNameCol   db "  |  Name: $"
ledgerBalCol    db "  |  Balance: $"

; NOTE: totalAssetsMsg intentionally ends mid-line
;       so printNumber output follows flush on same line
totalAssetsMsg  db 10,13
db "============================================================",10,13
db "            TOTAL BANK ASSET LIQUIDITY: $"

totalAssetsEnd  db 10,13
db "============================================================$"

; =====================================================
; TRANSFER LIST LABELS
; =====================================================

transferListHeader db 10,13
db "--- REGISTERED ACCOUNTS ---",10,13,"$"
transferAccText    db "  Name: $"

; =====================================================
; GENERAL UI
; =====================================================

pauseMsg db 10,13
db "------------------------------------------------------------",10,13
db "Press Any Key To Continue...$"

newline db 10,13,"$"
sepLine db 10,13,"------------------------------------------------------------$"

; =====================================================
; MULTI-USER DATABASE  (MAX 5 Users)
; =====================================================

MAX_USERS equ 5

; Core account arrays
accountNo_arr dw MAX_USERS dup(0)
userPin_arr   dw MAX_USERS dup(0)
balance_arr   dw MAX_USERS dup(0)

; Transaction history arrays
historyType_arr db MAX_USERS dup('N')
historyAmt_arr  dw MAX_USERS dup(0)
historyPeer_arr dw MAX_USERS dup(0)

; Name store: 5 slots x 20 bytes = 100 bytes
userName_arr db 100 dup('$')

; =====================================================
; SYSTEM CONTROL VARIABLES
; =====================================================

adminPin         dw 0
isAdminPinSet    db 0
userCount        dw 0
currentUserIndex dw 0

; Scratch variable: holds grand total across int 21h calls
grandTotal       dw 0

tempName db 20 dup('$')
choice   db ?
attempts db 0

.code

; =====================================================
; MAIN PROGRAM
; =====================================================

main proc

    mov ax,@data
    mov ds,ax

    ; --------------------------------------------------
    ; BOOT SEQUENCE
    ; --------------------------------------------------

    call clearScreen

    mov ah,09h
    lea dx,loadingMsg
    int 21h

    mov ah,09h
    lea dx,newline
    int 21h

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    ; --------------------------------------------------
    ; FIRST-TIME ADMIN PIN SETUP CHECK
    ; --------------------------------------------------

    cmp isAdminPinSet,1
    je start                        ; already configured this session

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    mov ah,09h
    lea dx,adminPromptMsg
    int 21h

    ; Read 4 masked digits into BX
    mov bx,0
    mov cx,4

setupPinLoop:

    mov ah,08h
    int 21h

    mov dl,'*'
    mov ah,02h
    int 21h

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov dx,10
    mul dx
    mov bx,ax

    pop ax
    add bx,ax

    loop setupPinLoop

    ; Lock the pin in permanently for this session
    mov adminPin,bx
    mov isAdminPinSet,1

    mov ah,09h
    lea dx,newline
    int 21h

    mov ah,09h
    lea dx,successMsg
    int 21h

    call pause

; =====================================================
; MAIN MENU ROUTER
; =====================================================

start:

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    mov ah,09h
    lea dx,mainMenu
    int 21h

    mov ah,01h
    int 21h

    mov choice,al

    cmp choice,'1'
    je register

    cmp choice,'2'
    je login

    cmp choice,'3'
    je exitProgram

    cmp choice,'4'
    je adminMode

    jmp start

; =====================================================
; REGISTER ACCOUNT
; =====================================================

register:

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    mov ax,userCount
    cmp ax,MAX_USERS
    jae systemFull

    ; --- Read Name ---
    mov ah,09h
    lea dx,nameMsg
    int 21h

    lea si,tempName
    mov cx,19

nameLoop:

    mov ah,01h
    int 21h

    cmp al,13
    je nameDone

    cmp cx,0
    je nameDone

    mov [si],al
    inc si
    dec cx

    jmp nameLoop

nameDone:

    mov byte ptr [si],'$'

    ; --- Read Account Number ---
    mov ah,09h
    lea dx,accMsg
    int 21h

    mov bx,0

accLoop:

    mov ah,01h
    int 21h

    cmp al,13
    je accDone

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov cx,10
    mul cx
    mov bx,ax

    pop ax
    add bx,ax

    jmp accLoop

accDone:

    ; --- Duplicate check ---
    mov cx,userCount
    cmp cx,0
    je noDuplicate

    mov si,0

dupCheckLoop:

    mov di,si
    shl di,1

    cmp bx,accountNo_arr[di]
    je duplicateAccount

    inc si
    loop dupCheckLoop

noDuplicate:

    mov si,userCount
    shl si,1
    mov accountNo_arr[si],bx

    ; --- Read PIN ---
    mov ah,09h
    lea dx,pinMsg
    int 21h

    mov bx,0
    mov cx,4

pinLoop:

    mov ah,08h
    int 21h

    mov dl,'*'
    mov ah,02h
    int 21h

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov dx,10
    mul dx
    mov bx,ax

    pop ax
    add bx,ax

    loop pinLoop

    mov si,userCount
    shl si,1
    mov userPin_arr[si],bx

    ; --- Read Initial Deposit ---
    mov ah,09h
    lea dx,initialDepositMsg
    int 21h

    mov bx,0

initLoop:

    mov ah,01h
    int 21h

    cmp al,13
    je initDone

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov cx,10
    mul cx
    mov bx,ax

    pop ax
    add bx,ax

    jmp initLoop

initDone:

    mov si,userCount
    shl si,1
    mov balance_arr[si],bx

    ; Initialise history
    mov si,userCount
    mov historyType_arr[si],'D'

    mov di,userCount
    shl di,1
    mov historyAmt_arr[di],bx
    mov historyPeer_arr[di],0

    ; --- Copy name into userName_arr slot ---
    mov ax,userCount
    mov cx,20
    mul cx
    mov di,ax

    lea si,tempName

copyName:

    mov al,[si]
    mov userName_arr[di],al
    cmp al,'$'
    je copyNameDone
    inc si
    inc di
    jmp copyName

copyNameDone:

    inc userCount

    mov ah,09h
    lea dx,successMsg
    int 21h

    call pause
    jmp start

systemFull:

    mov ah,09h
    lea dx,systemFullMsg
    int 21h

    call pause
    jmp start

duplicateAccount:

    mov ah,09h
    lea dx,duplicateMsg
    int 21h

    call pause
    jmp start

; =====================================================
; LOGIN
; =====================================================

login:

    mov attempts,0

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    mov ah,09h
    lea dx,loginAccMsg
    int 21h

    mov bx,0

l1:

    mov ah,01h
    int 21h

    cmp al,13
    je l2

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov cx,10
    mul cx
    mov bx,ax

    pop ax
    add bx,ax

    jmp l1

l2:

    mov cx,userCount
    cmp cx,0
    je fail

    mov si,0

searchAccLoop:

    mov di,si
    shl di,1

    cmp bx,accountNo_arr[di]
    je accFound

    inc si
    loop searchAccLoop

    jmp fail

accFound:

    mov currentUserIndex,si

    mov ah,09h
    lea dx,loginPinMsg
    int 21h

    mov bx,0
    mov cx,4

lp:

    mov ah,08h
    int 21h

    mov dl,'*'
    mov ah,02h
    int 21h

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov dx,10
    mul dx
    mov bx,ax

    pop ax
    add bx,ax

    loop lp

    mov si,currentUserIndex
    shl si,1

    cmp bx,userPin_arr[si]
    jne fail

    mov attempts,0

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    mov ah,09h
    lea dx,holderText
    int 21h

    mov ax,currentUserIndex
    mov cx,20
    mul cx
    mov si,ax

    lea dx,userName_arr
    add dx,si

    mov ah,09h
    int 21h

    call pause
    jmp userPanel

fail:

    inc attempts

    cmp attempts,3
    je blocked

    mov ah,09h
    lea dx,loginFailMsg
    int 21h

    call pause
    jmp start

blocked:

    mov ah,09h
    lea dx,blockedMsg
    int 21h

    call pause
    jmp exitProgram

; =====================================================
; ADMIN MODE
; =====================================================

adminMode:

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    mov ah,09h
    lea dx,adminLoginMsg
    int 21h

    mov bx,0
    mov cx,4

adminPinEntry:

    mov ah,08h
    int 21h

    mov dl,'*'
    mov ah,02h
    int 21h

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov dx,10
    mul dx
    mov bx,ax

    pop ax
    add bx,ax

    loop adminPinEntry

    cmp bx,adminPin
    jne adminFail

; =====================================================
; ADMIN CONTROL PANEL ROUTER
; =====================================================

adminPanel:

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    mov ah,09h
    lea dx,adminMenuText
    int 21h

    mov ah,01h
    int 21h

    mov choice,al

    cmp choice,'1'
    je viewLedger

    cmp choice,'2'
    je viewReserves

    cmp choice,'3'
    je exitAdmin

    jmp adminPanel

adminFail:

    mov ah,09h
    lea dx,loginFailMsg
    int 21h

    call pause
    jmp start

exitAdmin:

    jmp start

; =====================================================
; ADMIN [1] - MASTER LEDGER
; =====================================================

viewLedger:

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    mov ah,09h
    lea dx,ledgerHeader
    int 21h

    mov di,0

ledgerLoop:

    cmp di,userCount
    jge ledgerDone

    mov ah,09h
    lea dx,ledgerRowPrefix
    int 21h

    mov si,di
    shl si,1
    mov ax,accountNo_arr[si]
    call printNumber

    mov ah,09h
    lea dx,ledgerNameCol
    int 21h

    mov ax,di
    mov cx,20
    mul cx
    mov si,ax

    lea dx,userName_arr
    add dx,si

    mov ah,09h
    int 21h

    mov ah,09h
    lea dx,ledgerBalCol
    int 21h

    mov si,di
    shl si,1
    mov ax,balance_arr[si]
    call printNumber

    inc di
    jmp ledgerLoop

ledgerDone:

    mov ah,09h
    lea dx,newline
    int 21h

    call pause
    jmp adminPanel

; =====================================================
; ADMIN [2] - TOTAL BANK ASSET LIQUIDITY  (FIXED)
; =====================================================
; TECHNICAL NOTE:
;   int 21h (ah=09h) destroys AX after execution.
;   The running total is therefore accumulated in SI
;   throughout the entire loop — SI is NOT used by
;   int 21h and is safe across DOS calls.
;   Only when the loop is fully complete is the total
;   moved from SI into AX for printNumber, with no
;   int 21h call between that move and the print.
;   grandTotal (dw) acts as a second safety bridge so
;   the label print cannot corrupt the value.
; =====================================================

viewReserves:

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    ; --- Accumulate grand total into SI ---
    ; SI is preserved across int 21h; AX is not.
    mov si,0                    ; SI = running total (starts at 0)
    mov di,0                    ; DI = loop index   (starts at 0)

reserveLoop:

    cmp di,userCount            ; have we processed every user?
    jge reserveDone             ; yes -> exit loop

    ; Scale index: word offset = DI * 2
    mov bx,di
    shl bx,1                    ; BX = DI * 2

    ; Read this user's balance and add to running total
    mov ax,balance_arr[bx]      ; AX = balance of user at index DI
    add si,ax                   ; SI = SI + balance  (accumulate)

    inc di                      ; advance to next user
    jmp reserveLoop

reserveDone:

    ; SI now holds the exact live sum of all user balances.
    ; Save it to grandTotal BEFORE any int 21h touches AX.
    mov grandTotal,si

    ; Print the label string
    mov ah,09h
    lea dx,totalAssetsMsg
    int 21h                     ; AX is destroyed here — SI still safe

    ; Reload grand total from memory into AX for printNumber
    ; (int 21h cannot reach between these two lines)
    mov ax,grandTotal
    call printNumber            ; prints the correct live total

    ; Print closing border
    mov ah,09h
    lea dx,totalAssetsEnd
    int 21h

    mov ah,09h
    lea dx,newline
    int 21h

    call pause
    jmp adminPanel

; =====================================================
; USER PANEL ROUTER
; =====================================================

userPanel:

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    mov ah,09h
    lea dx,userMenu
    int 21h

    mov ah,01h
    int 21h

    mov choice,al

    cmp choice,'1'
    je deposit

    cmp choice,'2'
    je withdraw

    cmp choice,'3'
    je showBalance

    cmp choice,'4'
    je showDetails

    cmp choice,'5'
    je viewHistory

    cmp choice,'6'
    je changePin

    cmp choice,'7'
    je transferMoney

    cmp choice,'8'
    je logout

    jmp userPanel

; =====================================================
; DEPOSIT
; =====================================================

deposit:

    call clearScreen

    mov ah,09h
    lea dx,depositMsg
    int 21h

    mov bx,0

d1:

    mov ah,01h
    int 21h

    cmp al,13
    je d2

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov cx,10
    mul cx
    mov bx,ax

    pop ax
    add bx,ax

    jmp d1

d2:

    mov si,currentUserIndex
    shl si,1
    add balance_arr[si],bx

    mov si,currentUserIndex
    mov historyType_arr[si],'D'

    mov di,currentUserIndex
    shl di,1
    mov historyAmt_arr[di],bx
    mov historyPeer_arr[di],0

    mov ah,09h
    lea dx,successMsg
    int 21h

    call pause
    jmp userPanel

; =====================================================
; WITHDRAW
; =====================================================

withdraw:

    call clearScreen

    mov ah,09h
    lea dx,withdrawMsg
    int 21h

    mov bx,0

w1:

    mov ah,01h
    int 21h

    cmp al,13
    je w2

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov cx,10
    mul cx
    mov bx,ax

    pop ax
    add bx,ax

    jmp w1

w2:

    mov si,currentUserIndex
    shl si,1

    cmp balance_arr[si],bx
    jb insufficient

    sub balance_arr[si],bx

    mov si,currentUserIndex
    mov historyType_arr[si],'W'

    mov di,currentUserIndex
    shl di,1
    mov historyAmt_arr[di],bx
    mov historyPeer_arr[di],0

    mov ah,09h
    lea dx,successMsg
    int 21h

    call pause
    jmp userPanel

; =====================================================
; TRANSFER MONEY  (with live balance indicator)
; =====================================================
; UPGRADE NOTE:
;   After the receiver account is validated and before
;   the transfer amount prompt, the sender's live
;   balance is read from balance_arr[currentUserIndex]
;   and displayed via printNumber.  The balance is
;   saved into grandTotal first so the label print
;   (int 21h) cannot destroy the value before it is
;   shown — the same pattern used in viewReserves.
; =====================================================

transferMoney:

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    ; --- Display beneficiary list ---

    mov ah,09h
    lea dx,transferListHeader
    int 21h

    mov di,0

showBeneficiaryLoop:

    cmp di,userCount
    jge showBeneficiaryDone

    cmp di,currentUserIndex
    je skipSelf

    mov ah,09h
    lea dx,newline
    int 21h

    mov si,di
    shl si,1
    mov ax,accountNo_arr[si]
    call printNumber

    mov ah,09h
    lea dx,transferAccText
    int 21h

    mov ax,di
    mov cx,20
    mul cx
    mov si,ax

    lea dx,userName_arr
    add dx,si

    mov ah,09h
    int 21h

skipSelf:

    inc di
    jmp showBeneficiaryLoop

showBeneficiaryDone:

    mov ah,09h
    lea dx,newline
    int 21h

    ; --- Enter receiver account number ---

    mov ah,09h
    lea dx,transferAccMsg
    int 21h

    mov bx,0

t1:

    mov ah,01h
    int 21h

    cmp al,13
    je t2

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov cx,10
    mul cx
    mov bx,ax

    pop ax
    add bx,ax

    jmp t1

t2:

    cmp userCount,0
    je invalidReceiver

    ; --- Search for receiver ---

    mov si,0

searchRecvLoop:

    cmp si,userCount
    jge invalidReceiver

    mov di,si
    shl di,1

    cmp bx,accountNo_arr[di]
    je recvFound

    inc si
    jmp searchRecvLoop

recvFound:

    ; Reject self-transfer
    cmp si,currentUserIndex
    je invalidReceiver

    ; Save receiver index on stack
    push si

    ; ==================================================
    ; LIVE BALANCE DISPLAY (NEW)
    ; Show sender's current funds BEFORE amount prompt.
    ; Save to grandTotal first; label print destroys AX.
    ; ==================================================

    mov si,currentUserIndex
    shl si,1
    mov ax,balance_arr[si]      ; AX = sender's live balance
    mov grandTotal,ax           ; save before int 21h kills AX

    mov ah,09h
    lea dx,senderBalMsg         ; print "Your Available Balance: "
    int 21h                     ; AX destroyed here — grandTotal safe

    mov ax,grandTotal           ; reload saved balance
    call printNumber            ; print the correct live amount

    mov ah,09h
    lea dx,newline
    int 21h

    mov ah,09h
    lea dx,sepLine
    int 21h

    ; ==================================================
    ; END LIVE BALANCE DISPLAY
    ; ==================================================

    ; --- Enter transfer amount ---

    mov ah,09h
    lea dx,transferMsg
    int 21h

    mov cx,0

t3:

    mov ah,01h
    int 21h

    cmp al,13
    je t4

    sub al,48
    mov ah,0

    push ax

    mov ax,cx
    mov dx,10
    mul dx
    mov cx,ax

    pop ax
    add cx,ax

    jmp t3

t4:

    ; CX = transfer amount

    ; Check sender balance
    mov si,currentUserIndex
    shl si,1

    cmp balance_arr[si],cx
    jb transferInsufficient

    ; Deduct from sender
    sub balance_arr[si],cx

    ; Collect both account numbers before writing history
    pop si                          ; SI = receiver index
    push si

    mov di,si
    shl di,1
    mov dx,accountNo_arr[di]        ; DX = receiver acc no

    mov di,currentUserIndex
    shl di,1
    mov bx,accountNo_arr[di]        ; BX = sender acc no

    ; --- Update SENDER history ---
    mov di,currentUserIndex
    mov historyType_arr[di],'T'

    shl di,1
    mov historyAmt_arr[di],cx
    mov historyPeer_arr[di],dx

    ; --- Credit receiver ---
    pop si

    mov di,si
    shl di,1
    add balance_arr[di],cx

    ; --- Update RECEIVER history ---
    mov historyType_arr[si],'R'
    mov historyAmt_arr[di],cx
    mov historyPeer_arr[di],bx

    mov ah,09h
    lea dx,successMsg
    int 21h

    call pause
    jmp userPanel

transferInsufficient:

    pop si

    mov ah,09h
    lea dx,insufficientMsg
    int 21h

    call pause
    jmp userPanel

invalidReceiver:

    mov ah,09h
    lea dx,invalidRecMsg
    int 21h

    call pause
    jmp userPanel

; =====================================================
; SHOW BALANCE
; =====================================================

showBalance:

    call clearScreen

    mov ah,09h
    lea dx,balanceText
    int 21h

    mov si,currentUserIndex
    shl si,1

    mov ax,balance_arr[si]
    call printNumber

    mov ah,09h
    lea dx,newline
    int 21h

    call pause
    jmp userPanel

; =====================================================
; ACCOUNT DETAILS
; =====================================================

showDetails:

    call clearScreen

    mov ah,09h
    lea dx,detailsText
    int 21h

    mov ah,09h
    lea dx,holderText
    int 21h

    mov ax,currentUserIndex
    mov cx,20
    mul cx
    mov si,ax

    lea dx,userName_arr
    add dx,si

    mov ah,09h
    int 21h

    mov ah,09h
    lea dx,accountText
    int 21h

    mov si,currentUserIndex
    shl si,1

    mov ax,accountNo_arr[si]
    call printNumber

    mov ah,09h
    lea dx,newline
    int 21h

    mov ah,09h
    lea dx,availableText
    int 21h

    mov si,currentUserIndex
    shl si,1

    mov ax,balance_arr[si]
    call printNumber

    mov ah,09h
    lea dx,newline
    int 21h

    call pause
    jmp userPanel

; =====================================================
; VIEW TRANSACTION HISTORY
; =====================================================

viewHistory:

    call clearScreen

    mov ah,09h
    lea dx,welcomeMsg
    int 21h

    mov ah,09h
    lea dx,historyHeader
    int 21h

    mov si,currentUserIndex
    mov al,historyType_arr[si]

    cmp al,'N'
    je histNone

    cmp al,'D'
    je histDeposit

    cmp al,'W'
    je histWithdraw

    cmp al,'T'
    je histSent

    cmp al,'R'
    je histReceived

    jmp histNone

histNone:

    mov ah,09h
    lea dx,noHistoryMsg
    int 21h

    call pause
    jmp userPanel

histDeposit:

    mov ah,09h
    lea dx,histTypeDepMsg
    int 21h

    mov si,currentUserIndex
    shl si,1
    mov ax,historyAmt_arr[si]
    call printNumber

    mov ah,09h
    lea dx,histPeerNone
    int 21h

    mov ah,09h
    lea dx,newline
    int 21h

    call pause
    jmp userPanel

histWithdraw:

    mov ah,09h
    lea dx,histTypeWthMsg
    int 21h

    mov si,currentUserIndex
    shl si,1
    mov ax,historyAmt_arr[si]
    call printNumber

    mov ah,09h
    lea dx,histPeerNone
    int 21h

    mov ah,09h
    lea dx,newline
    int 21h

    call pause
    jmp userPanel

histSent:

    mov ah,09h
    lea dx,histTypeSndMsg
    int 21h

    mov si,currentUserIndex
    shl si,1
    mov ax,historyAmt_arr[si]
    call printNumber

    mov bx,historyPeer_arr[si]  ; BX = receiver acc no

    mov ah,09h
    lea dx,histToAcc
    int 21h

    mov ax,bx
    call printNumber

    mov si,0

findSentPeerLoop:

    cmp si,userCount
    jge sentPeerDone

    mov di,si
    shl di,1

    cmp bx,accountNo_arr[di]
    je sentPeerFound

    inc si
    jmp findSentPeerLoop

sentPeerFound:

    mov ah,09h
    lea dx,histNameLabel
    int 21h

    mov ax,si
    mov cx,20
    mul cx
    mov si,ax

    lea dx,userName_arr
    add dx,si

    mov ah,09h
    int 21h

sentPeerDone:

    mov ah,09h
    lea dx,newline
    int 21h

    call pause
    jmp userPanel

histReceived:

    mov ah,09h
    lea dx,histTypeRcvMsg
    int 21h

    mov si,currentUserIndex
    shl si,1
    mov ax,historyAmt_arr[si]
    call printNumber

    mov bx,historyPeer_arr[si]  ; BX = sender acc no

    mov ah,09h
    lea dx,histFromAcc
    int 21h

    mov ax,bx
    call printNumber

    mov si,0

findRcvPeerLoop:

    cmp si,userCount
    jge rcvPeerDone

    mov di,si
    shl di,1

    cmp bx,accountNo_arr[di]
    je rcvPeerFound

    inc si
    jmp findRcvPeerLoop

rcvPeerFound:

    mov ah,09h
    lea dx,histNameLabel
    int 21h

    mov ax,si
    mov cx,20
    mul cx
    mov si,ax

    lea dx,userName_arr
    add dx,si

    mov ah,09h
    int 21h

rcvPeerDone:

    mov ah,09h
    lea dx,newline
    int 21h

    call pause
    jmp userPanel

; =====================================================
; CHANGE PIN
; =====================================================

changePin:

    call clearScreen

    mov ah,09h
    lea dx,loginPinMsg
    int 21h

    mov bx,0
    mov cx,4

oldPinLoop:

    mov ah,08h
    int 21h

    mov dl,'*'
    mov ah,02h
    int 21h

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov dx,10
    mul dx
    mov bx,ax

    pop ax
    add bx,ax

    loop oldPinLoop

    mov si,currentUserIndex
    shl si,1

    cmp bx,userPin_arr[si]
    jne wrongOldPin

    mov ah,09h
    lea dx,pinMsg
    int 21h

    mov bx,0
    mov cx,4

newPinLoop:

    mov ah,08h
    int 21h

    mov dl,'*'
    mov ah,02h
    int 21h

    sub al,48
    mov ah,0

    push ax

    mov ax,bx
    mov dx,10
    mul dx
    mov bx,ax

    pop ax
    add bx,ax

    loop newPinLoop

    mov si,currentUserIndex
    shl si,1

    mov userPin_arr[si],bx

    mov ah,09h
    lea dx,succesMsg
    int 21h

    call pause
    jmp userPanel

wrongOldPin:

    mov ah,09h
    lea dx,loginFailMsg
    int 21h

    call pause
    jmp userPanel

; =====================================================
; INSUFFICIENT BALANCE
; =====================================================

insufficient:

    mov ah,09h
    lea dx,insufficientMsg
    int 21h

    call pause
    jmp userPanel

; =====================================================
; LOGOUT
; =====================================================

logout:

    mov ah,09h
    lea dx,logoutMsg
    int 21h

    call pause
    jmp start

; =====================================================
; EXIT PROGRAM
; =====================================================

exitProgram:

    mov ah,4ch
    int 21h

main endp

; =====================================================
; CLEAR SCREEN PROCEDURE
; =====================================================

clearScreen proc

    mov ax,0003h
    int 10h

    ret

clearScreen endp

; =====================================================
; PAUSE PROCEDURE
; =====================================================

pause proc

    mov ah,09h
    lea dx,pauseMsg
    int 21h

    mov ah,01h
    int 21h

    ret

pause endp

; =====================================================
; PRINT NUMBER PROCEDURE
; =====================================================

printNumber proc

    mov cx,0
    mov bx,10

c1:

    mov dx,0
    div bx

    push dx

    inc cx

    cmp ax,0
    jne c1

c2:

    pop dx

    add dl,48

    mov ah,02h
    int 21h

    loop c2

    ret

printNumber endp

end main