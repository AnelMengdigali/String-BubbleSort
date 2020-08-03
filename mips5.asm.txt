.data
S: .asciiz "HelloWorld"
length: .word 10
nl: .asciiz "\n"
.text

la $a0, S
lw $a1, length

jal functPrint

li $v0, 4
la $a0, nl
syscall

move $a0, $t0	

jal string_bubble_sort
jal functPrint

li $v0, 10
syscall

string_bubble_sort:

	body:   
		addi $sp, $sp, -20
		sw $ra, 16($sp)
		sw $s3, 12($sp)
		sw $s2, 8($sp)
		sw $s1, 4($sp)
		sw $s0, 0($sp)
	 
		move $s2, $a0
		move $s3, $a1
		add $s0, $s0, $zero 
		
	Loop1:
		slt $t0, $s0, $s3 
		beq $t0, $zero, Exit1
		addi $s1, $s0, -1 
		
	Loop2:
		slti $t0, $s1, 0
		bne $t0, $zero, Exit2
		move $t1, $s1
		add $t2, $s2, $t1   
		lb $t3, 0($t2)
		lb $t4, 1($t2)
		slt $t0, $t4, $t3
		beq $t0, $zero, Exit2
		
		move $a0, $s2
		move $a1, $s1
		jal swap
		
		addi $s1, $s1, -1
		j Loop2
		
	Exit2:
		addi $s0, $s0, 1
		j Loop1
		
	swap:	
		move $t5, $a1
		add $t5, $a0, $t5
		
		lb $t0, 0($t5)
		lb $t2, 1($t5)
		
		sb $t2, 0($t5)
		sb $t0, 1($t5)
		
		jr $ra	
		
	Exit1:
		lw $s0, 0($sp)
		lw $s1, 4($sp)  
		lw $s2, 8($sp)
		lw $s3, 12($sp)
		lw $ra, 16($sp)
		addi $sp, $sp, 20
		
		jr $ra

	
functPrint:
	move $t0, $a0
	
	li $v0, 4
	move $a0, $t0
	syscall
	
	jr $ra
