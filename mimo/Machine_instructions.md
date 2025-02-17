
- izberi strojne ukaze in jih realiziraj
- realiziral sem vse ukaze - nahajajo se v "basic_microcode.def"

```
# A small set of microinstructions for the CPU
# (c) GPL3 Warren Toomey, 2012
#

fetch:	addrsel=pc irload=1 												# Address=PC, Load IR register
		pcload=1  pcsel=pc, opcode_jump										# PC=PC+1, jump to 2+OPC

# --------- ALU operations on Rd,Rs,Rt ---------
# add r1, r2, r3

0:		aluop=add  op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=+, ALU 2nd op = treg, write from aluout to reg
1:		aluop=sub op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=-, ALU 2nd op = treg, write from aluout to reg
2:		aluop=mul op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=*, ALU 2nd op = treg, write from aluout to reg
3:		aluop=div  op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=/, ALU 2nd op = treg, write from aluout to reg
4:		aluop=rem op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=rem, ALU 2nd op = treg, write from aluout to reg
5:		aluop=and op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=and, ALU 2nd op = treg, write from aluout to reg
6:		aluop=or  op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=or, ALU 2nd op = treg, write from aluout to reg
7:		aluop=xor op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=xor, ALU 2nd op = treg, write from aluout to reg
8:		aluop=nand op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=nand, ALU 2nd op = treg, write from aluout to reg
9:		aluop=nor  op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=nor, ALU 2nd op = treg, write from aluout to reg
10:		aluop=not op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=not, ALU 2nd op = treg, write from aluout to reg
11:		aluop=lsl op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=lsl, ALU 2nd op = treg, write from aluout to reg
12:		aluop=lsr op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=lsr, ALU 2nd op = treg, write from aluout to reg
13:		aluop=asr op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=asr, ALU 2nd op = treg, write from aluout to reg
14:		aluop=rol op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=rol, ALU 2nd op = treg, write from aluout to reg
15:		aluop=ror op2sel=treg  dwrite=1  regsrc=aluout, goto fetch			# ALU=ror, ALU 2nd op = treg, write from aluout to reg
# -------------------------------------------------------------


# --------- ALU operations on Rd,Rs,Immed ---------
# addi r1, r2, 5

16:		addrsel=pc imload=1
		aluop=add  op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=+, ALU 2nd op = immed, write from aluout to reg
17:		addrsel=pc imload=1
		aluop=sub op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=-, ALU 2nd op = immed, write from aluout to reg
18:		addrsel=pc imload=1
		aluop=mul op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=*, ALU 2nd op = immed, write from aluout to reg
19:		addrsel=pc imload=1
		aluop=div  op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=/, ALU 2nd op = immed, write from aluout to reg
20:		addrsel=pc imload=1
		aluop=rem op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=rem, ALU 2nd op = immed, write from aluout to reg
21:		addrsel=pc imload=1
		aluop=and op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=and, ALU 2nd op = immed, write from aluout to reg
22:		addrsel=pc imload=1
		aluop=or  op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=or, ALU 2nd op = immed, write from aluout to reg
23:		addrsel=pc imload=1
		aluop=xor op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=xor, ALU 2nd op = immed, write from aluout to reg
24:		addrsel=pc imload=1
		aluop=nand op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=nand, ALU 2nd op = immed, write from aluout to reg
25:		addrsel=pc imload=1
		aluop=nor  op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=nor, ALU 2nd op = immed, write from aluout to reg
26:		addrsel=pc imload=1
		aluop=lsl op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=lsl, ALU 2nd op = immed, write from aluout to reg
27:		addrsel=pc imload=1
		aluop=lsr op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=lsr, ALU 2nd op = immed, write from aluout to reg
28:		addrsel=pc imload=1
		aluop=asr op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=asr, ALU 2nd op = immed, write from aluout to reg
29:		addrsel=pc imload=1
		aluop=rol op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=rol, ALU 2nd op = immed, write from aluout to reg
30:		addrsel=pc imload=1
		aluop=ror op2sel=immed  dwrite=1  regsrc=aluout, goto pcincr		# ALU=ror, ALU 2nd op = immed, write from aluout to reg
# -------------------------------------------------------------


# --------- ALU operations on Rd,Rs,Rt,Immed ---------
# addc r1, r2, r3, immed (jump)

31: 	addrsel=pc imload=1
		aluop=add  op2sel=treg  dwrite=1  regsrc=aluout, if c then jump else pcincr
32: 	addrsel=pc imload=1
		aluop=sub  op2sel=treg  dwrite=1  regsrc=aluout, if c then jump else pcincr
# -------------------------------------------------------------

# --------- Jump compared to Rt ---------
# jeq Rs,Rt,immed  ; if Rs == Rt, PC <- immed else  PC <- PC + 2
33:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then jump else pcincr		    		# If z then jump to immed else pcincr; 

# jne Rs,Rt,immed  ; If Rs != Rt, PC <- immed else  PC <- PC + 2
34:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then pcincr else jump		    		# If z then pcincr else jump to immed; opsel=0 means value of 2 (constant 0 is selected!!!)

# jgt Rs,Rt,immed  ; If Rs > Rt, PC <- immed else  PC <- PC + 2
35:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then pcincr else posjump					# If z (Rs == 0) then pcincr


# jle Rs,Rt,immed  ; If Rs <= Rt, PC <- immed else  PC <- PC + 2
36:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then jump else negjump	    			# If z (Rs == 0) then jump

# jlt Rs,Rt,immed  ; If Rs < Rt, PC <- immed else  PC <- PC + 2	
37:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then pcincr else negjump					# If z (Rs == 0) then pcincr
	
# jge Rs,Rt,immed  ; If Rs >= Rt, PC <- immed else  PC <- PC + 2
38:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then jump else posjump	
# -------------------------------------------------------------


# --------- Jump compared to zero operations ---------
# jeqz Rs,immed  ; if Rs == 0, PC <- immed else  PC <- PC + 2
39:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then jump else pcincr		    		# If z then jump to immed else pcincr; 

# jnez Rs,immed  ; If Rs != 0, PC <- immed else  PC <- PC + 2
40:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then pcincr else jump		    		# If z then pcincr else jump to immed; opsel=0 means value of 2 (constant 0 is selected!!!)

# jgtz Rs,immed  ; If Rs > 0, PC <- immed else  PC <- PC + 2
41:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then pcincr else posjump					# If z (Rs == 0) then pcincr


# jlez Rs,immed  ; If Rs <= 0, PC <- immed else  PC <- PC + 2
42:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then jump else negjump	    			# If z (Rs == 0) then jump

# jltz Rs,immed  ; If Rs < 0, PC <- immed else  PC <- PC + 2	
43:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then pcincr else negjump					# If z (Rs == 0) then pcincr
	
# jgez Rs,immed  ; If Rs >= 0, PC <- immed else  PC <- PC + 2
44:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then jump else posjump	    			# If z (Rs == 0) then jump
# -------------------------------------------------------------

# --------- Jump to immed ---------
45: addrsel=pc  imload=1, goto jump
# ---------------------------------




# --------- Branch compared to Rt ---------
# beq Rs,Rt,immed  ; if Rs == Rt, PC <- PC + immed else  PC <- PC + 2
46:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then branch else pcincr		    		# If z then jump to immed else pcincr; 

# bne Rs,Rt,immed  ; If Rs != Rt, PC <- PC + immed else  PC <- PC + 2
47:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then pcincr else branch		    		# If z then pcincr else jump to immed; opsel=0 means value of 2 (constant 0 is selected!!!)

# bgt Rs,Rt,immed  ; If Rs > Rt, PC <- PC + immed else  PC <- PC + 2
48:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then pcincr else posbranch					# If z (Rs == 0) then pcincr


# ble Rs,Rt,immed  ; If Rs <= Rt, PC <- PC + immed else  PC <- PC + 2
49:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then branch else negbranch	    			# If z (Rs == 0) then jump

# blt Rs,Rt,immed  ; If Rs < Rt, PC <- PC + immed else  PC <- PC + 2	
50:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then pcincr else negbranch					# If z (Rs == 0) then pcincr
	
# bge Rs,Rt,immed  ; If Rs >= Rt, PC <- PC + immed else  PC <- PC + 2
51:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=treg, if z then branch else posbranch	
# -------------------------------------------------------------


# --------- Branch compared to zero operations ---------
# beqz Rs,immed  ; if Rs == 0, PC <- PC + immed else  PC <- PC + 2
52:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then branch else pcincr		    		# If z then jump to immed else pcincr; 

# bnez Rs,immed  ; If Rs != 0, PC <- PC + immed else  PC <- PC + 2
53:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then pcincr else branch		    		# If z then pcincr else jump to immed; opsel=0 means value of 2 (constant 0 is selected!!!)

# bgtz Rs,immed  ; If Rs > 0, PC <- PC + immed else  PC <- PC + 2
54:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then pcincr else posbranch					# If z (Rs == 0) then pcincr


# blez Rs,immed  ; If Rs <= 0, PC <- PC + immed else  PC <- PC + 2
55:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then branch else negbranch    			# If z (Rs == 0) then jump

# bltz Rs,immed  ; If Rs < 0, PC <- PC + immed else  PC <- PC + 2	
56:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then pcincr else negbranch				# If z (Rs == 0) then pcincr
	
# bgez Rs,immed  ; If Rs >= 0, PC <- PC + immed else  PC <- PC + 2
57:	addrsel=pc  imload=1								            		# Read Immediate operand -> IMRegister
	aluop=sub  op2sel=const0, if z then branch else posbranch	    			# If z (Rs == 0) then jump
# -------------------------------------------------------------

# --------- Branch to immed ---------
58: addrsel=pc  imload=1, goto branch
# ---------------------------------

# ---------- Stack ----------------
# init -> li r7, 0x3FFF

# jsr immed (jump to immed) 
59: addrsel=pc  imload=1
	aluop=sub op2sel=const1  swrite=1  regsrc=aluout # R7 -- 
	pcload=1  pcsel=pc	# PC + 1			
	addrsel=sreg  datawrite=1  datasel=pc, goto jump

# rts - return 
60: addrsel=sreg  imload=1
	pcload=1  pcsel=immed # return 
	aluop=add  op2sel=const1  swrite=1  regsrc=aluout, goto fetch # R7++

# --------- Increment / dicrement ---------
# inc Rs (Rs <- Rs + 1)
61: aluop=add  op2sel=const1  swrite=1  regsrc=aluout, goto fetch
# dec Rs (Rs <- Rs - 1)
62: aluop=sub  op2sel=const1  swrite=1  regsrc=aluout, goto fetch

# li Rd,Immed    ; Load immediate Rd, immed   
63:	addrsel=pc  dwrite=1  regsrc=databus, goto pcincr						# additional increment (cause immed was read) and then new command
#	pcload=1  pcsel=pc, goto fetch                                  		# not optimal way to do this, better goto to pcincr: instead (line above)

# lw Rd, immed ;Rd <- M[immed]
64: addrsel=pc  imload=1
    addrsel=immed  dwrite=1  regsrc=databus, goto pcincr

# sw Rd,immed    ; Store Rd into M[immed] Rd->M[immed]; 
65:	addrsel=pc  imload=1
	addrsel=immed  datawrite=1  datasel=dreg, goto pcincr					# additional increment (cause immed was read) and then new command
#	pcload=1  pcsel=pc, goto fetch                                  		# not optimal way to do this, better goto to pcincr: instead (line above)

# lwi Rd,Rs,immed ; Rd <- M[Rs+immed]
66: addrsel=pc  imload=1
	aluop=add  op2sel=immed addrsel=aluout  dwrite=1  regsrc=databus, goto pcincr

# swi Rd,Rs,immed ; M[Rs+immed] <- Rd
67: addrsel=pc  imload=1
	aluop=add  op2sel=immed addrsel=aluout  datawrite=1  datasel=dreg, goto pcincr

# ------------ PUSH / POP ---------------
#push Rd
68: aluop=sub op2sel=const1  swrite=1  regsrc=aluout
	addrsel=sreg  datawrite=1  datasel=dreg, goto fetch

#pop Rd
69: addrsel=sreg dwrite=1  regsrc=databus
	aluop=add op2sel=const1  swrite=1  regsrc=aluout, goto fetch

# move Rd,Rs
70: dwrite=1  regsrc=sreg, goto fetch

# clr - MUL 0, and - pobrise z 0
71: aluop=and  op2sel=const0  swrite=1  regsrc=aluout, goto fetch

# neg - obrnemo 15 bit (XOR), MUL -1, NOT 
72: aluop=not  op2sel=const1  swrite=1  regsrc=aluout, goto fetch

# lwri Rd,Rs,Rt ; Rd <- M[Rs+Rt]
73: aluop=add  op2sel=treg addrsel=aluout  dwrite=1  regsrc=databus, goto fetch

# swri Rd,Rs,Rt ; M[Rs+Rt] <- Rd
74: aluop=add  op2sel=treg addrsel=aluout  datawrite=1  datasel=dreg, goto fetch


#
# Other code which we call
#
# Increment PC and goto new command; for all commands that use immediate operand - they need additional PC increment !
pcincr:	pcload=1  pcsel=pc, goto fetch

# Set address to immed and goto new command; for jumps to absolute immed address
jump:	pcload=1  pcsel=immed, goto fetch

# Set address to PC + immed and goto new command; for branch to relative immed address
branch:	pcload=1  pcsel=pcimmed, goto fetch

# jump if positive (!n)
posjump:	aluop=sub  op2sel=const0, if n then pcincr else jump
# jump if negative (n)
negjump:	aluop=sub  op2sel=const0, if n then jump else pcincr	

# branch if positive (!n)
posbranch:	aluop=sub  op2sel=const0, if n then pcincr else branch
# branch if negative (n)
negbranch:	aluop=sub  op2sel=const0, if n then branch else pcincr

```