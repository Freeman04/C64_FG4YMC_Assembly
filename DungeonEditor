; 10 SYS (2304)

*=$0801

     BYTE $0E, $08, $0A, $00, $9E, $20, $28,  $32, $33, $30, $34, $29, $00, $00, $00

*=$0900

; ------------------------
; Declarations / Mapping
;-------------------------

Border     = $d020
Background = $d021

ScrnChar   = $0400
ScrnColor  = $d800

EdgeColor  = $0854
FillColor  = $0855

BoxWidth   = $0856
BoxHeight  = $0857

BoxX       = $8058
BoxY       = $8059

; ------------------------
; Main Loop
;-------------------------


                lda #147        ;  Clear
                jsr $ffd2       ;  Screen

                lda #$00        ; Set Backgrounds
                sta Border
                sta Background
        
                ; Make Title Box

                sta BoxX
                sta BoxY

                lda #$07        
                sta EdgeColor
                lda #$02
                sta FillColor

                lda #$28
                sta BoxWidth
                lda #$05
                sta BoxHeight

                jsr DrawBox

                ; Make Editing Box

                lda #$00
                sta BoxX
                lda #$05        ; set startin
                sta BoxY

                lda #$02
                sta EdgeColor
                lda #$01
                sta FillColor

                lda #$11
                adc BoxX
                sta BoxWidth
                lda #$11
                adc BoxY
                sta BoxHeight

                jsr DrawBox

                ; place text

                jsr write

main            lda #$00
                jsr main

Exit            rts

; ------------------------
; Subroutines
; ------------------------

; ------------------------
; Write Text at a location

write           ldx #$00
                
                ; Load Terminator Character

@_write         lda header1,x
                and #$3f        ; Converts to Uppercase
                clc             ; Clear incoming carry
                adc #$80        ; Flips characters.  But is always 1 higher on the first pass.
                sta $0429,x


                inx
                cpx #$26

                ; Look for terminator character then rmove cpx above.

                bne @_write

@quitWrite      rts

; ------------------------
; Draw a box at a location

DrawBox         lda #$00                        
                sta @DrawBox+1                
                lda #$04                       
                sta @DrawBox+2

                lda #$00                        
                sta @ColorBox+1                
                lda #$d8                       
                sta @ColorBox+2

                lda #$00
                sta counter
                
@Header         lda counter             ; What line are we on?
                clc
                cmp BoxY                ; Compare it to starting line
                beq @BoxLoop            ; go to write stuff if we're good
                
                jmp @BoxNewLine         ; We need a new line

                jmp @Header             ; Check new line.


@BoxLoop        lda #$A0
                ldx BoxX
                ldy counter

@DrawBox        sta ScrnChar,x

                lda EdgeColor

                cpx BoxX                ; if first line of x,
                beq @ColorBox         ; go ahead and use edge color

                cpy BoxY                ; if first column of y,
                beq @ColorBox         ; go ahead and use edge color

                inx                   ; I need to see if they're at the edge
                iny

                cpx BoxWidth
                beq @reset

                cpy BoxHeight
                beq @reset

                lda FillColor            ; if no conditions are 1, use fill color.

@reset          dex
                dey

@ColorBox       sta ScrnColor,x

                lda #$A0

                inx

                cpx BoxWidth
                beq @BoxNewLine

                cpx #40
                bne @DrawBox

@BoxNewLine     clc                             
                lda #40                    
                adc @DrawBox+1                
                sta @DrawBox+1                
                lda #00                    
                adc @DrawBox+2                
                sta @DrawBox+2

                clc                             
                lda #40                    
                adc @ColorBox+1                
                sta @ColorBox+1                
                lda #00                    
                adc @ColorBox+2                
                sta @ColorBox+2

                inc counter 

                lda counter
                cmp BoxHeight
                beq @QuitBox

                cmp BoxY
                bcc @Header

                cmp #25
                bne @BoxLoop

@QuitBox        rts

; ------------------------
; Data
; ------------------------
        
header1         text 'LEVEL GENERATOR '
header2         text 'THIS IS LEVEL: '
header3         text 'PRESS H FOR HELP '

width           byte 40
height          byte 5

counter         byte 0
