
#### Initialization
##### 1. Mode register (DBGU_MR)
- nacin delovanja --> CHMODE (bit 14,15)
- CHMODE (normal mode = 00) <-- kabel 
- CHMODE (local loopback mode = 10) <-- brez kabla
- parity --> PAR (bit 9,10,11)
- PAR (No parity -> 1,x,x) - brez paritete
- PAR (Even parity -> 0,0,0) - soda pariteta
- PAR (Odd parity -> 0,0,1) - liha pariteta
##### 2. Baud rate generator register (DBGU_BRGR)
- hitrost poslijnaja/sprejemanja - boudrate 
- hitrost prenosa 1 signalnega elementa (1 bit)
- CD - clock divisor
- CD = 0 --> OFF
- CD = 1 --> MCK 
- x = boudrate (19200 Hz)
- CD =  MCK / (x * 16)
- CD = 48 MHz / (19.2 KHz * 16) = 156.25 ~ 156
##### 3. Control register (DBGU_CR)
- Enable receiver and transmitter
- Receiver enable (bit4 = 1)
- Transmitter enable (bit6 = 1)
-------------------------------------------------------------
#### Transmit character - SEND
##### 1. Status register (DBGU_SR)
- Transmitter ready (1 = character has been trasmitted) <-- TXRDY (bit 1)
##### 2. Transmit holding register (DBGU_THR)
- Write - only
- Character to be trasmittted - 8bit (TXCHR)
----------------------------------------------------------------
#### Receive character
##### 1. Status register (DBGU_SR)
- Receiver ready (1 = character has been received) <-- RXRDY (bit 0)
##### 2. Receive holding register (DBGU_RHR)
- Ready - only
- Received character - 8bit (RXCHR)

##### Addresses
```
	.equ DBGU_BASE, 0xFFFFF200  /* Debug Unit Base Address */
      .equ DBGU_CR,   0x00        /* DBGU Control Register */
      .equ DBGU_MR,   0x04        /* DBGU Mode Register*/
      .equ DBGU_IER,  0x08        /* DBGU Interrupt Enable Register*/
      .equ DBGU_IDR,  0x0C        /* DBGU Interrupt Disable Register */
      .equ DBGU_IMR,  0x10        /* DBGU Interrupt Mask Register */
      .equ DBGU_SR,   0x14        /* DBGU Status Register */
      .equ DBGU_RHR,  0x18        /* DBGU Receive Holding Register */
      .equ DBGU_THR,  0x1C        /* DBGU Transmit Holding Register */
      .equ DBGU_BRGR, 0x20        /* DBGU Baud Rate Generator Register */
```

##### DEBUG_INIT
```
DEBUG_INIT: 
      stmfd r13!, {r0-r2, lr}
      
	  ldr r0, =DBGU_BASE
	  mov r1, #0b10 << 14         /* CHMODE (bit 14,15 = 10) */  
      mov r2, #1 << 11            /* Parity (bit 9,10,11 = 1xx */
      add r1, r1, r2
	  str r1, [r0, #DBGU_MR]	    /* DBGU Mode register */
      
      mov r1, #156                /* Clock divisor - 156 for (19200 Hz boud rate) */
      str r1, [r0, #DBGU_BRGR]    /* Boud rate generator register */     
  
      mov r1, #0b101 << 4         /* Enable RCV, TRX (bit 4,6) */
      str r1, [r0, #DBGU_CR]      /* DBGU Control register */
      
      ldmfd r13!, {r0-r2, pc}
```
##### MAIN
```
      bl DEBUG_INIT
      mov r0, #'a'
      
LOOP: bl SND_DEBUG
      bl RCV_DEBUG
      add r0, r0, #1
      b LOOP
```
##### RCV_DEBUG
```
RCV_DEBUG: 
      stmfd r13!, {r1,r2, lr}
      
      ldr r1, =DBGU_BASE
waitr:ldr r2, [r1, #DBGU_SR]        /* Debug Status register */       
      tst r2, #1                    /* RXRDY -> bit 0 */           
      beq waitr
      
      ldrb r0, [r1, #DBGU_RHR]      /* Load character from Receive Holding Register */
      ldmfd r13!, {r1,r2, pc}
```
##### SND_DEBUG
```
SND_DEBUG:
      stmfd r13!, {r1,r2, lr}
      
      ldr r1, =DBGU_BASE
waits:ldr r2, [r1, #DBGU_SR]        /* Debug Status register */       
      tst r2, #2                    /* TXRDY -> bit 1 */           
      beq waits
      
      strb r0, [r1, #DBGU_THR]      /* Load character into Transmit Holding Register */
      ldmfd r13!, {r1,r2 pc}
```