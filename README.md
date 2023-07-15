.data
     colorTable:  
                  .word 0x00000000    #BLACK = 0
                  .word 0x00FF0000    #Red = 1
                  .word 0x0000FF00    #Green = 2
                  .word 0x00FFFF00    #Yellow = 3
                  .word 0x000000FF    #Blue = 4
                  .word 0xFFFFFFFF    #White = 5
      #Game level digits
      digit1:     .asciiz "1"
      digit2:     .asciiz "2"
      digit3:     .asciiz "3"
      digit4:     .asciiz "4"  
      #STRINGS  
      introduction: .asciiz "Welcome to Simon Says. Please enter 1 for easy, 2 for normal, 3 for hard. If you wish to leave enter 0"
      colorPrompt:  .asciiz "RED: 1\nGREEN: 2\nYELLOW: 3\nBLUE: 4\n"
      invalidPrompt:.asciiz "Input invalid please try another!"
      winPrompt:    .asciiz "You win!"
      losePrompt:   .asciiz "You lose!"
      aSequence:    .space 100
      max:          .word 0
      ID:           .word 0
      randomNumber: .word 0
      seedGen:      .word 0
      #STACK
      beginStack:
      		    .word 0 : 40
      endStack:
      DigitTable:
                .byte   ' ', 0,0,0,0,0,0,0,0,0,0,0,0
                .byte   '0', 0x7e,0xff,0xc3,0xc3,0xc3,0xc3,0xc3,0xc3,0xc3,0xc3,0xff,0x7e
                .byte   '1', 0x38,0x78,0xf8,0x18,0x18,0x18,0x18,0x18,0x18,0x18,0x18,0x18
                .byte   '2', 0x7e,0xff,0x83,0x06,0x0c,0x18,0x30,0x60,0xc0,0xc1,0xff,0x7e
                .byte   '3', 0x7e,0xff,0x83,0x03,0x03,0x1e,0x1e,0x03,0x03,0x83,0xff,0x7e
                .byte   '4', 0xc3,0xc3,0xc3,0xc3,0xc3,0xff,0x7f,0x03,0x03,0x03,0x03,0x03
                .byte   '5', 0xff,0xff,0xc0,0xc0,0xc0,0xfe,0x7f,0x03,0x03,0x83,0xff,0x7f
                .byte   '6', 0xc0,0xc0,0xc0,0xc0,0xc0,0xfe,0xfe,0xc3,0xc3,0xc3,0xff,0x7e
                .byte   '7', 0x7e,0xff,0x03,0x06,0x06,0x0c,0x0c,0x18,0x18,0x30,0x30,0x60
                .byte   '8', 0x7e,0xff,0xc3,0xc3,0xc3,0x7e,0x7e,0xc3,0xc3,0xc3,0xff,0x7e
                .byte   '9', 0x7e,0xff,0xc3,0xc3,0xc3,0x7f,0x7f,0x03,0x03,0x03,0x03,0x03
                .byte   '+', 0x00,0x00,0x00,0x18,0x18,0x7e,0x7e,0x18,0x18,0x00,0x00,0x00
                .byte   '-', 0x00,0x00,0x00,0x00,0x00,0x7e,0x7e,0x00,0x00,0x00,0x00,0x00
                .byte   '*', 0x00,0x00,0x00,0x66,0x3c,0x18,0x18,0x3c,0x66,0x00,0x00,0x00
                .byte   '/', 0x00,0x00,0x18,0x18,0x00,0x7e,0x7e,0x00,0x18,0x18,0x00,0x00
                .byte   '=', 0x00,0x00,0x00,0x00,0x7e,0x00,0x7e,0x00,0x00,0x00,0x00,0x00
                .byte   'A', 0x18,0x3c,0x66,0xc3,0xc3,0xc3,0xff,0xff,0xc3,0xc3,0xc3,0xc3
                .byte   'B', 0xfc,0xfe,0xc3,0xc3,0xc3,0xfe,0xfe,0xc3,0xc3,0xc3,0xfe,0xfc
                .byte   'C', 0x7e,0xff,0xc1,0xc0,0xc0,0xc0,0xc0,0xc0,0xc0,0xc1,0xff,0x7e
                .byte   'D', 0xfc,0xfe,0xc3,0xc3,0xc3,0xc3,0xc3,0xc3,0xc3,0xc3,0xfe,0xfc
                .byte   'E', 0xff,0xff,0xc0,0xc0,0xc0,0xfe,0xfe,0xc0,0xc0,0xc0,0xff,0xff
                .byte   'F', 0xff,0xff,0xc0,0xc0,0xc0,0xfe,0xfe,0xc0,0xc0,0xc0,0xc0,0xc0
# add additional characters here....
# first byte is the ascii character
# next 12 bytes are the pixels that are "on" for each of the 12 lines
        .byte    0, 0,0,0,0,0,0,0,0,0,0,0,0
      
.text

   main: 
	la  $sp, endStack   #Stack
#diagonal line One
        la $a0, 30
        la $a1, 70
        la $a2, 5
        la $a3, 200
        jal drawHorizontalLine
#diagonal line two
	la $a0, 30
        la $a1, 220
        la $a2, 5
        la $a3, 200
        jal drawHorizontalLine
#red Circle
	la $a0, 130	#x coordinate
	la $a1, 60	#y coordinate
	la $a2, 1	#color
	la $a3, 32	#size 
      redFillLoop:
	jal  drawCircle		
	addi $a3, $a3, -1	#Decrement radius
	bnez $a3, redFillLoop	#If a3 is not 0, branch
		
       #Digit for red circle
        li $a0, 3            #X location
        li $a0, 120          #Y location  
        la $a2, digit1
        jal OutText
        
        #Tone for red circle
        li $a0, 50            #Pitch
        li $a1, 5000          #Duration of tone
        li $a2, 0             
        li $a3, 125           #volume
        li $v0, 31            #Prompt to tone for circle
        syscall	
        		
#green Circle
	la  $a0, 50		#x coordniate
	la  $a1, 130		#y coordinate
	la  $a2, 2		#color
	la  $a3, 32		#square size 
       greenFillLoop:
	jal drawCircle		
	addi $a3, $a3, -1		#Decrement radius
	bnez  $a3, greenFillLoop	#If a3 is not 0, branch	
	
	#Digit for green circle
	li $a0, 40            #X location
	li $a1, 120             #Y location
	la $a2, digit2
	jal OutText
	
	#Tone for green circle
	li $a0, 65		#Pitch
	li $a1, 5000		#Duration of tone
	li $a2, 0	       
	li $a3, 125		#Volume
	li $v0, 31		
	syscall	
				
#Yellow Circle
	la $a0, 220	#x coordinate
	la $a1, 130	#y coordinate
	la $a2, 3	#color
	la $a3, 32	#size 
      yellowFillLoop:
	jal   drawCircle		
	addi  $a3, $a3, -1		#Decrement radius
	bnez  $a3, yellowFillLoop	#If a3 is not 0, branch
	
	#Digit for yellow circle
	li $a0, 220		#x location
	li $a1, 130		#y location
	la $a2, digit3
	jal OutText
		
	#Tone for yellow circle
	li $a0, 70		#Pitch
	li $a1, 5000		#Duration of tone
	li $a2, 0		
	li $a3, 125		#Volume
	li $v0, 31		
	syscall	
							
#Blue Circle
	la  $a0, 130		#x coordinate
	la  $a1, 220		#y coordniate
	la  $a2, 4		#color  
	la  $a3, 32		#size 
      blueFillLoop:
	jal  drawCircle		#Jump and link to drawCircle
	addi $a3, $a3, -1		#Decrement radius
	bnez $a3, blueFillLoop	#If a3 is not 0, branch
	
	#Digit for blue circle
	li $a0, 120		#x location
	li $a1, 215		#y location
	la $a2, digit4
	jal OutText
		
	#tone for blue circle
	li $a0, 80		#Pitch
	li $a1, 5000		#Duration of tone
	li $a2, 0		#Instrument
	li $a3, 125		#Volume
	li $v0, 31			
	syscall			
	
	la $a0, aSequence #load  the sequence into $a0
	la $a1, max       #Load max into $a1
	
	jal initializeValues #clear all values 
	
	la $a0, introduction
	li $v0, 4          #Prompt for the introduction statement
	syscall
	
	li $v0, 5          #users imput
	syscall
	
	#check the users skill leavel input
	beq $v0, 0, exit		#Branch if $v0 is equal to quit
	beq $v0, 1, easyLevel		#Branch if $v0 is equal to easy
	beq $v0, 2, normalLevel		#Branch if $v0 is equal to normal
	beq $v0, 3, hardLevel		#Branch if $v0 is equal to hard
	j invalid		        #Jump to invalid
	
     easyLevel:
         li $t0, 5      #$t0 = 5
         sw $t0, max    #Store
         j cont         #continue 
         
     normalLevel:
	li $t0, 8	#8 into $t0
	sw $t0, max	#Store
	j cont		#continue
	
     hardLevel:
	li $t0, 11      #11 into $t0
	sw $t0, max	#Store 5 into max label	
	j cont		#continue
	
    cont:	
	la $a0, colorPrompt	#colorPrompt into $a0
	li $v0, 4		#Load print string syscall
	syscall					
	
	jal clearDisplay		
	#pause
	li $a0, 800		#Sleep
	li $v0, 32		
	syscall					
	#Difficulty Loop
	li $t6, 0		#Counter
	lw $t5, max		#Number of sequences
   generateSequenceLoop:
	#Arguments
	la $a0, ID
	la $a1, seedGen		#Load address of seed into $a1

	jal getRandomNumber		
	sw $v0, randomNumber	#Store generated number
		
	#Load Arguments
	la $a0, aSequence	#Load address of sequence into $a0
	la $a1, randomNumber	#Load address of randomNumber into $a1
	
	#Sequence Order
	move $t7, $t6		#counter into $t1
	sll  $t7, $t7, 2	#Multiply by 4
	add  $a0, $a0, $t7	#Add array address to correct element
	addi $t6, $t6, 1	#Increment counter
	
	#Check the loop and add to the sequence
	jal addToSequence		
	bne $t6, $t5, generateSequenceLoop	#Loop if counter is not max
	
	#Load the aruguments
	la $a0, aSequence	#Load address of seqArray into $a0
	la $a1, max		#Load address of max into $a1
	
	jal displaySequence	
	li $v0, 1		#$v0 to win
	
   printResult:	
	beq $v0, 0, lose	#equal to 0
	beq $v0, 1, win		#equal to 1
	
     invalid:
	la  $a0, invalidPrompt	#Load address of invalid prompt
	li  $v0, 4			#Load print string syscall
	syscall					
	
	li   $v0, 11		#Load print character syscall
	addi $a0, $0, 0xA	#Load ascii character for newline into $a0
	syscall			
	j    reset
			
      lose:
	la $a0, losePrompt	#Load lose prompt
	li $v0, 4		#Load print string syscall
	syscall		
	
	li $v0, 11		#Load print character
	addi $a0, $0, 0xA	#Load ascii character for newline
	syscall					
	j reset	
		
      win:
	la $a0, winPrompt	#Load win promp
	li $v0, 4		#Load print string syscall
	syscall					
	
	li $v0, 11		#Load print character
	addi $a0, $0, 0xA	#Load ascii character for newline
	syscall					
	j reset
		
     reset:
	#Reset and Loop
	li $a0, 0
	li $a1, 0
	li $a2, 0
	li $s1, 0
	li $s2, 0
	li $s3, 0
	li $t0, 0
	li $t1, 0
	li $t2, 0
	li $t3, 0
	li $t4, 0
	li $t5, 0
	li $t6, 0
	li $v0, 0
	j main
			
     exit:
	li $v0, 17	
	syscall				
#Procedure: initializeValues
#clear all values to start a new game
#a0 is pointed to the array
#a1 pointed to max
   initializeValues:
	li $t0, 24	#Clear max
	li $t1, 0	#Index
     initializeLoop:
	sb   $0, 0($a0)		       #Clear index
	addi $t1, $t1, 1	       
	addi $a0, $a0, 4	        #increase to next element
	bne  $t1, 25, initializeLoop	#Loop if counter is not 100
	
	sw $0, 0($a1)		#Reset
	jr $ra			
#procedure: getRandomNumber
#Random Number is generated for the sequence
#a0 is pointer 
#a1 pointer to seed
#v0 is the generated	
  getRandomNumber:
        sw $0, 0($a0)           #ID = 0
	move $t0, $a0		#save ID in $a0 into $t0
	move $t1, $a1		#save seed in $a1 into $t1
	
	li $v0, 30		#Load syscall for systime
	syscall					
	sw $a0, 0($t1)		#Store systime into seed
	#Seed Generator
	lw $a0, ($t0)		#Set $a0 to genID
	lw $a1, ($t1)		#Set $a1 to seed
	li $v0, 40	        #Load syscall for seed
	syscall					
	sw $a1, 0($t1)		#Store generated seed into label
	
	li $a0, 10    #pause
	li $v0, 32 
	syscall
	#Random set from 1 to 4
	li $a1, 4		#range = 4
	li $v0, 42		#Load syscall for random int range
	syscall		
	addi $a0, $a0, 1	#Add 1 to make range 1-4
	move $v0, $a0		#random to $v0
	#Reset
	move $a0, $t0		#genID in $t0 into $a0
	move $a1, $t1		#seed in $t1 into $a1
	jr $ra			
#procedure: addToSequence
##add random number to the sequence
#a0 = pointer to array
#a1 pointer to random number	
  addToSequence:
	lw $t0, 0($a1)		#randomNumber into $t0
	sb $t0, 0($a0)		#Store into sequence	
	jr $ra			
#procedure: displaySequence
#Generate the sequence to the user
#a0 pointer to array
#a1 pointer to max
  displaySequence:
	#Space and Save
	addi $sp, $sp, -12	#Space for 4 words
	sw $ra, 0($sp)		#Store $ra = 0 
	sw $a0, 4($sp)		#Store $a0 = 1
	sw $a1, 8($sp)		#Store $a1 = 2
	lw $s3, 0($a1)		#max into $s3
	
	li $s2, 2		#loop one by one number of elements to display
	move $s5, $a0		#Save pointer to first element
	lw $t1, 0($a1)		#max from $a1
     forLoop:	
        beqz $s1, checkSkip	
	#Reset
	li $a0, 0
	li $a1, 0			
	lw $a0, 4($sp)		#Reset $a0 
	lw $a1, 8($sp)		#Reset $a1 
	
	addi $s2, $s2, 1	#Increment counter 2 by 1
	sw $s2, 0($a1)		#max into label
	jal userCheck		
	
	#Reset
	li $a0, 0			
	li $a1, 0			
	lw $s5, 4($sp)		#Reset $s5
	lw $a0, 4($sp)		#Reset $a0 address
	move $t1, $s2		#max from $s2
	
	beq $s2, $s3, displayDone	#If loop does not equal max

	li $a0, 1200		#pause
	li $v0, 32	        #Load syscall for sleep
	syscall					
	move $a0, $s5		#Reset $a0
    checkSkip:
	#Blink
	li $s1, 0		#Counter
	move $t2, $a0		#sequence to $t2
	move $t4, $a1		#max to $t4
	
	jal clearDisplay	#Clear Display
	move $t2, $s5		#Load sequence
     blinkLoop:	
	lb $t3, 0($t2)		#Load element
	move $a0, $t3		#Copy number to blink
	li $a1, 700		#Reset $a1
	jal blinkNumber
	move $t2, $s5		#Load sequence	
    returnLoop:				
	addi $t2, $t2, 4	#Increment to next element
	addi $s1, $s1, 1	#Increment counter by 1		
	move $s5, $t2		#Set $s5

	li $a0, 800		#pause
	li $v0, 32		#Load syscall for sleep
	syscall					
	
	ble  $s1, $s2, blinkLoop	#Loop if counter has not reached 2nd counter	
	j forLoop
    displayDone:
	#Restore
	lw $ra, 0($sp)		#Restore
	addi $sp, $sp, 4	#Read
	jr $ra		         
#procedure: userCheck
#Generate the sequence to the user
#a0 is the pointer to the sequence array
#a1 is the pointer to max	
  userCheck:
	#Space and Save
	addi $sp, $sp, -12	#Space for 4 words
	sw $ra, 0($sp)		#Store $ra =0
	sw $a1, 4($sp)		#Store max = 1 
	sw $a0, 8($sp)		#Store max = 2
	#Blink
	li $s4, 0		#Counter	
	move $s6, $a0		#sequence to $t2
     userCheckLoop:
        li  $v0, 12
        syscall				
	
	addi $v0, $v0, -48	#Convert ascii to decimal
	lb $a0, 0($s6)		#Get element from sequence
	bne $v0, $a0, failed	#Check if correct
	
	addi $s6, $s6, 4 #Increment to next element
	addi $s4, $s4, 1 #Increment counter by 1
	#Loop and blink
	li $a1, 70		#Reset 
	jal blinkNumber		
	lw $a1, 4($sp)		#Load max pointer of stack
	lw $a1, 0($a1)		#Load max of stack
	bne $s4, $a1, userCheckLoop	#Loop if counter is not max
	li $v0, 1			#Set return win
	
	lw $ra, 0($sp)		#Restore
	addi $sp, $sp, 12	#Read
	jr $ra			
       failed:
       #Sound played when fail
        li  $a0, 0		#Pitch
	li  $a1, 4000		#Duration
	li  $a2, 0		#Instrument
	li  $a3, 127		#Volume
	li  $v0, 31			
	syscall	
					
	li $a0, 1200		#pause
	li $v0, 32		#Load syscall for sleep
	syscall		
	
	lb   $a0, 0($s6)	#Get element from sequence
	li   $a1, 200		#pause
	jal  blinkNumber	
	lb   $a0, 0($s6)	#Get element from sequence
	li   $a1, 200		#pause
	jal  blinkNumber	
	
	li $v0, 0		#Set return to Lose
	j printResult		
#Procedure: drawDot
#Dots are drawn on the display
#a0 = x
#a1 = y
#a2 = color
  drawDot:
	addi $sp, $sp, -8	#Space for 2 words
	sw $a2, 0($sp)		#Store $a2 = 0
	sw $ra, 4($sp)		#Store $ra  = 1
	
	jal calculateAddress
	lw  $a2, 0($sp)		#Restore $a2
	sw  $v0, 0($sp)		#Save $v0 on element 0 
	
	jal getColor		#Color in $v1
	lw $v0, 0($sp)		#Restores $v0
	
	#Create dot
	sw $v1, 0($v0)		#Make dot
	lw $ra, 4($sp)		#Restore $ra 
	addi $sp, $sp, 8	#Read
	jr $ra			
#procedure: calculateAddress
#Covert x & y to address
#a0 = x
#a1 = y
#v0 = memory	
  calculateAddress:
	sll $a0, $a0, 2		        #$a0 * 4
	sll $a1, $a1, 7                 #a1 * 256
	sll $a1, $a1, 3		        #$a1 * 4
	add $a0, $a0, $a1		#Add $a1 to $a0
	addi $v0, $a0, 0x10040000	#Add base address for display + $a0 = $v0	
	jr $ra		
#procedure: getColor
#Based on a2 get the color
#a2= color	
  getColor:	
	la $a0, colorTable	
	sll $a2, $a2, 2		#Index x4 is offset
	add $a2, $a2, $a0	#base + offset
	lw $v1, 0($a2)		#color from memory
	jr $ra			
#Procedure: drawHorizontalLine	
#Horizontal Lines are drawn on the bitmap display
#a0 = x
#a1 = y
#a2 = color
#a3 = length
  drawHorizontalLine:
	addi $sp, $sp, -16	#Space for 4 words
	sw $ra, 12($sp)		#Store $ra = 4
	sw $a0, 0($sp)		#Store $a0  = 0
	sw $a1, 4($sp)		#Store $a1 = 1
	sw $a2, 8($sp)		#Store $a2 = 2
      horizontalLoop:
	jal drawDot	
	#Restore
	lw $a0, 0($sp)		#Restore $a0
	lw $a1, 4($sp)		#Restore $a1
	lw $a2, 8($sp)		#Restore $a2
	
	addi $a0, $a0, 1	        #Increment by 1
	sw $a0, 0($sp)		        #Store $a0 on element 0 
	addi $a3, $a3, -1	        #Decrement 
	bne $a3, $0, horizontalLoop	#If length is not 0
	
	lw $ra, 12($sp)		#Restore $ra 
	addi $sp, $sp, 16	
	jr $ra			
#procedure: drawVerticalLine
#Vertical lines are drawn on the bitmap display
#a0 = x
#a1 = y
#a2 = color
#a3 = length
   drawVerticalLine:
	addi $sp, $sp, -16	#Space  for 4 words
	sw $ra, 12($sp)		#Store $ra 
	sw $a0, 0($sp)		#Store $a0 
	sw $a1, 4($sp)		#Store $a0 
	sw $a2, 8($sp)		#Store $a2 	
      verticalLoop:
	jal drawDot		
	
	lw $a0, 0($sp)		#Restore $a0 
	lw $a1, 4($sp)		#Restore $a1 
	lw $a2, 8($sp)		#Restore $a2 
	
	addi $a1, $a1, 1	   #Increment y by 1
	sw $a1, 4($sp)		   #Store $a1 on element 1
	addi $a3, $a3, -1	   #Decrement length of line
	bne $a3, $0, verticalLoop  #If length is not 0, loop
	
	lw $ra, 12($sp)		#Restore $ra 
	addi $sp, $sp, 16		
	jr $ra			
#Procedure: drawBox
#Draw boxes on the bitmap
#a0 = x coordinate
#a1 = y coordinate
#a2 = color 
#a3 = size
  drawBox:
	addi $sp, $sp, -24	# make space for 5 words
	sw $ra, 12($sp)		#Store $ra on element 4 
	sw $a0, 0($sp)		#Store $a0 on element 0
	sw $a1, 4($sp)		#Store $a1 on element 1
	sw $a2, 8($sp)		#Store $a2 on element 2
	sw $a3, 20($sp)		#Store $a3 on element 5 
	move $s0, $a3		#Copy $a3 to temp
	sw $s0, 16($sp)		#Store $s0 on element 5
    boxLoop:
	jal drawHorizontalLine
	#restor the registers
	lw  $a0, 0($sp)		
	lw  $a1, 4($sp)		
	lw  $a2, 8($sp)		
	lw  $a3, 20($sp)		
	lw  $s0, 16($sp)		

	addi $a1, $a1, 1	#Increment y by 1
	sw $a1, 4($sp)		#Store $a1 on element 1 
	addi $s0, $s0, -1	#Decrement counter
	sw $s0, 16($sp)		#Store $s0 on element 5 
	bne $s0, $0, boxLoop	#If length is not 0
        #Restore and reset
	lw $ra, 12($sp)		#Restore $ra 
	addi $sp, $sp, 24	#Read
	addi $s0, $s0, 0	#Reset
	jr $ra		
#procedure: clearDisplay
#Clear on the bitmap	
clearDisplay:
	addi $sp, $sp, -4	#Spacefor 1 words
	sw $ra, 0($sp)		#Store $ra 
	
	la $a0, 0        #x-coordinate 
	la $a1, 0        #y-coordinate 
	la $a2, 0        #color = black
	la $a3, 256	 #size to clear = 32	
	jal drawBox		#Clear Screen
	
	lw $ra, 0($sp)		#Restore
	addi $sp, $sp, 4	#Readjust stack	
	jr $ra	
#Procedure: blinkNumber
#Clear on bitmap
#a0 = number that is blinked
#a1 = pause	
blinkNumber:
	addi $sp, $sp, -8	# make space for 2 words
	sw $ra, 0($sp)		#Store $ra 
	sw $a1, 4($sp)		#Store $a1 

	beq $a0, 1, playRed	
	beq $a0, 2, playGreen	
	beq $a0, 3, playYellow	
	beq $a0, 4, playBlue	

  blinkCRed:
    playRed:	
       #tone 
	li $a0, 60
	li $a1, 1000
	li $a2, 0
	li $a3, 127
	li $v0, 31
	syscall
	
	#drawing
	la $a0, 130
	la $a1, 60
	la $a2, 1
	la $a3, 32
	
      redFillLoop2:
	jal drawCircle		
	addi $a3, $a3, -1	#Decrement radius
	bnez $a3, redFillLoop2	#If a3 is not 0
	#digit
	li  $a0, 120			
	li  $a1, 80			
	la  $a2, digit1
	jal OutText
	
	lw $a0, 4($sp)		#pause
	li $v0, 32		#Load syscall for sleep
	syscall		

	la $a0, 130	#x 
	la $a1, 60	#y 
	la $a2, 0	#color
	la $a3, 32	#size
     redFillLoop3:
	jal drawCircle		
	addi $a3, $a3, -1	    #Decrement radius
	bnez $a3, redFillLoop3    #If a3 is not 0
	
	#pause
	lw $a0, 4($sp)		
	li $v0, 32	    
	syscall					

	la $a0, 130	#x 
	la $a1, 60	#y 
	la $a2, 0	#color
	la $a3, 32	#size
     redFillLoop4:
	jal drawCircle		
	addi $a3, $a3, -1	  #Decrement radius
	bnez $a3, redFillLoop4  #If a3 is not 0, branch
	j continueUserCheck
	
   playGreen:
       #Tone for Green
	li $a0, 65		#Pitch
	li $a1, 1000		#Duration
	li $a2, 0		#Instrument
	li $a3, 127		#Volume
	li $v0, 31			
	syscall	
				
	la $a0, 50		#x 
	la $a1, 130		#y 
	la $a2, 2	        #color 
	la $a3, 32		#size 
     greenFillLoop2:
	jal drawCircle		
	addi $a3, $a3, -1	#Decrement radius
	bnez $a3, greenFillLoop2	#If a3 is not 0
	#Digit
	li  $a0, 40		
	li  $a1, 120			
	la  $a2, digit2
	jal OutText

	lw $a0, 4($sp)		#pause
	li $v0, 32		#Load syscall for sleep
	syscall					
	
	la $a0, 50		#x
	la $a1, 130		#y 
	la $a2, 0		#color
	la $a3, 32		#size
    greenFillLoop3:
	jal drawCircle		
	addi $a3, $a3, -1	  #Decrement radius
	bnez $a3, greenFillLoop3	  #If a3 is not 0
	
	lw $a0, 4($sp)		#pause
	li $v0, 32		#Load syscall for sleep
	syscall				
        #blink for green
	la $a0, 50		#x 
	la $a1, 130		#y 
	la $a2, 0		#color
	la $a3, 32	 	#size 
    greenFillLoop4:
	jal drawCircle		
	addi $a3, $a3, -1  	 #Decrement radius
	bnez $a3, greenFillLoop4	 #If a3 is not 0
	j    continueUserCheck
	
  playYellow:
       #Tone for yellow
	li  $a0, 75	#Pitch
	li  $a1, 1000	#Duration
	li  $a2, 0	#Instrument
	li  $a3, 127	#Volume
	li  $v0, 31	
	syscall	
							
    blinkCYellow:
	la $a0, 220	#x
	la $a1, 130	#y 
	la $a2, 3	#color
	la $a3, 32	#square size
      yellowFillLoop2:
	jal drawCircle		
	addi $a3, $a3, -1		#Decrement radius
	bnez $a3, yellowFillLoop2	#If a3 is not 0, branch
	#Digit
	li  $a0, 220			
	li  $a1, 130		
	la  $a2, digit3
	jal OutText

	lw $a0, 4($sp)		#pause
	li $v0, 32		#Load syscall for sleep
	syscall			
        #Blink for yellow
	la $a0, 220	
	la $a1, 130	
	la $a2, 0			
	la $a3, 32			
     yellowFillLoop3:
	jal drawCircle		
	addi $a3, $a3, -1		#Decrement radius
	bnez $a3, yellowFillLoop3	#If a3 is not 0

	lw $a0, 4($sp)		#pause
	li $v0, 32		#Load syscall 
	syscall	
	#Blink for yellow				
	la $a0, 220			
	la $a1, 130		
	la $a2, 0			
	la $a3, 32			
	yellowFillLoop4:
	jal drawCircle		
	addi $a3, $a3, -1       #Decrement radius
	bnez $a3, yellowFillLoop4	#If a3 is not 0	
	j continueUserCheck
	
  playBlue:
       #Tone for blue
	li  $a0, 70	#Pitch
	li  $a1, 1000	#Duration
	li  $a2, 0	#Instrument
	li  $a3, 127	#Volume
	li  $v0, 31	
	syscall	
    blinkCBlue:
	la $a0, 130	#x 
	la $a1, 220     #y 
	la $a2, 4	#color
	la $a3, 32	#size
     blueFillLoop2:
	jal drawCircle		
	addi $a3, $a3, -1	#Decrement radius
	bnez $a3, blueFillLoop2	#If a3 is not 0
	#Digit
	li $a0, 120
	li $a1, 215
	la $a2, digit4
	jal OutText
	
	lw $a0, 4($sp)		#pause
	li $v0, 32		#Load syscall for sleep
	syscall			
	
	la $a0, 130		#x 
	la $a1, 220		#y 
	la $a2, 0		#color
	la $a3, 32		#size
      blueFillLoop3:
	jal  drawCircle		
	addi $a3, $a3, -1	#Decrement radius
	bnez $a3, blueFillLoop3	#If a3 is not 0
		
	lw $a0, 4($sp)		#pause
	li $v0, 32		#Load syscall for sleep
	syscall				
        #Blink for blue
	la $a0, 130		#x 
	la $a1, 220		#y 
	la $a2, 0		#color
	la $a3, 32	        #size 
     blueFillLoop4:
	jal drawCircle		
	addi $a3, $a3, -1	#Decrement radius
	bnez $a3, blueFillLoop4	#If a3 is not 0
	j continueUserCheck
		
    continueUserCheck:
	lw   $ra, 0($sp)	#Restore
	addi $sp, $sp, 8	#Read
	jr   $ra
#procedure: getCharacter
#wait for input
getCharacter:
	addi  $sp, $sp, -4	#Make sapce for 1 words
	sw    $ra, 0($sp)	#Store $ra on element 0
	li    $s7, 0		#Counter
	j     check	 	#Skip	
    characterLoop:
	#sleep
	li  $a0, 1000		#number one, second
	li  $v0, 32	        #Load syscall for sleep
	syscall	
    check:
	jal   characterT		#Jump and link
	addi  $s7, $s7, 1		#Increment
	beq   $s7, 10, leaveCharacter	#If 5 seconds have passed, leave
	beqz  $v0, characterLoop	#If input, finish polling, else loop	
     leaveCharacter:
	lui   $t0, 0xffff		#Register
	lw    $v0, 4($t0)		#Get control	
	#Restore and reset registers
	lw    $ra, 0($sp)		#Restore
	addi  $sp, $sp, 4		#Read
	jr    $ra
#Procedure: characterT
#Wait for input
#$v0 = no data
#v0 = 1 character in the buffer
characterT:
       	lui  $t0, 0xffff	#Register
	lw   $t1, 0($t0)	#control
	and  $v0, $t1, 1	#least significent bit
	jr   $ra
#Procedure: drawCircle
#Drawing the circle in the center
#$a0 = x(0)
#$a1 = y(0)
#a2 = color
#a3 = radius
drawCircle:
       addi  $sp, $sp, -20   #Make space for one word
       sw    $ra, 0($sp)     #$ra will be stored in 0 in stack
       sw    $a0, 4($sp)     #$a0 will be stored in 1 of stack
       sw    $a1, 8($sp)     #$a1 will be stored in 2 of stack
       sw    $a2, 12($sp)    #a2 will be stored in 3 of stack
       sw    $a3, 16($sp)    #a3 will be stored in 4 of stack
       
       move $t0, $a0	    #x(0)
       move $t1, $a1	    #y(0)
       move $t2, $a3	    #radius
       addi $t3, $t2, -1    #x
       li   $t4, 0	    #y
       li   $t5, 1	    #dx of x
       li   $t6, 1	    #dy of y
       li   $t7, 0	    #Error
       #if(dx - (radius << 1))
       sll  $t8, $t2, 1     #Check and calculate for errors
       subu $t7, $t5, $t8   #Subtract dx of x from the shifted radius
    cirLoop:     #while loop: x >= y
       blt $t3, $t4 skipCircle    #when y is greater than x, skip the cirLoop
       #Dots
       addu  $a0, $t0, $t4         #Draw at coordinates (x(0) + y , y(0) + x)
       addu  $a1, $t1, $t3
       lw    $a2, 12($sp)
       jal   drawDot		   #Jump and link
       
       addu  $a0, $t0, $t3         #Draw at coordinates (x(0) + x , y(0) + y)
       addu  $a1, $t1, $t4
       lw    $a2, 12($sp)
       jal   drawDot		   #Jump and link
       
       subu $a0, $t0, $t3           #Draw at coordinates (x(0) - x , y(0) + y)
       addu $a1, $t1, $t4
       lw   $a2, 12($sp)
       jal  drawDot	
       
       subu  $a0, $t0, $t4         #Draw at coordinates (x(0) - y , y(0) + x)
       addu  $a1, $t1, $t3
       lw    $a2, 12($sp)
       jal   drawDot
       
       subu  $a0, $t0, $t3        #Draw at coordinates (x(0) - x , y(0) - y)
       subu  $a1, $t1, $t4
       lw    $a2, 12($sp)
       jal   drawDot
       
       addu  $a0, $t0, $t4         #Draw at coordinates (x(0) + y , y(0) - x)
       subu  $a1, $t1, $t3
       lw    $a2, 12($sp)
       jal   drawDot
       
       subu  $a0, $t0, $t4         #Draw at coordinates (x(0) - y , y(0) - x)
       subu  $a1, $t1, $t3
       lw    $a2, 12($sp)
       jal   drawDot
       
       addu  $a0, $t0, $t3         #Draw at coordinates (x(0) + x , y(0) - y)
       subu  $a1, $t1, $t4
       lw    $a2, 12($sp)
       jal   drawDot
       #Error less than or equal to 0
       bgtz  $t7, else
       addi  $t4, $t4, 1		#y++
       addu  $t7, $t7, $t6		#error += dy of y
       addi  $t6, $t6, 2		#dy of y += 2
       j     circleContinue		#Skip and jump
       #else if error is greater than 0
    else:
       addi  $t3, $t3, -1		#x--
       addi  $t5, $t5, 2		#dx of x += 2
       sll   $t8, $t2, 1		#radius left 1	
       subu  $t9, $t5, $t8		#Subtract dx - radius 
       addu  $t7, $t7, $t9		#error += $t9
    circleContinue:
       j     cirLoop
       
    skipCircle:
       #Restore and reset registers
	lw   $ra, 0($sp)		
	lw   $a0, 4($sp)		
	lw   $a1, 8($sp)		
	lw   $a2, 12($sp)		
	lw   $a3, 16($sp)		
	addi $sp, $sp, 20		#Read
	jr   $ra		
# OutText: display ascii characters on the bit mapped display
# $a0 = horizontal pixel co-ordinate (0-255)
# $a1 = vertical pixel co-ordinate (0-255)
# $a2 = pointer to asciiz text (to be displayed)
OutText:
        addiu   $sp, $sp, -24
        sw      $ra, 20($sp)

        li      $t8, 1          # line number in the digit array (1-12)
_text1:
        la      $t9, 0x10040000 # get the memory start address
        sll     $t0, $a0, 2     # assumes mars was configured as 256 x 256
        addu    $t9, $t9, $t0   # and 1 pixel width, 1 pixel height
        sll     $t0, $a1, 10    # (a0 * 4) + (a1 * 4 * 256)
        addu    $t9, $t9, $t0   # t9 = memory address for this pixel

        move    $t2, $a2        # t2 = pointer to the text string
_text2:
        lb      $t0, 0($t2)     # character to be displayed
        addiu   $t2, $t2, 1     # last character is a null
        beq     $t0, $zero, _text9

        la      $t3, DigitTable # find the character in the table
_text3:
        lb      $t4, 0($t3)     # get an entry from the table
        beq     $t4, $t0, _text4
        beq     $t4, $zero, _text4
        addiu   $t3, $t3, 13    # go to the next entry in the table
        j       _text3
_text4:
        addu    $t3, $t3, $t8   # t8 is the line number
        lb      $t4, 0($t3)     # bit map to be displayed

        sw      $zero, 0($t9)   # first pixel is black
        addiu   $t9, $t9, 4

        li      $t5, 8          # 8 bits to go out
_text5:
        la      $t7, colorTable
        lw      $t7, 0($t7)     # assume black
        andi    $t6, $t4, 0x80  # mask out the bit (0=black, 1=white)
        beq     $t6, $zero, _text6
        la      $t7, colorTable     # else it is white
        lw      $t7, 4($t7)
_text6:
        sw      $t7, 0($t9)     # write the pixel color
        addiu   $t9, $t9, 4     # go to the next memory position
        sll     $t4, $t4, 1     # and line number
        addiu   $t5, $t5, -1    # and decrement down (8,7,...0)
        bne     $t5, $zero, _text5

        sw      $zero, 0($t9)   # last pixel is black
        addiu   $t9, $t9, 4
        j       _text2          # go get another character

_text9:
        addiu   $a1, $a1, 1     # advance to the next line
        addiu   $t8, $t8, 1     # increment the digit array offset (1-12)
        bne     $t8, 13, _text1

        lw      $ra, 20($sp)
        addiu   $sp, $sp, 24
        jr      $ra
		
		
		
	
  
