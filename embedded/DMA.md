#### 1. Branje niza
- Zacetni naslov niza --> RPR (Receive Pointer Register)
- Stevilo znakov niza --> RCR (Receive Counter Register)
- Zacetek DMA (prejemanje) --> RXTEN (enable, bit 0) v PTCR (Periph. Transfer Control Register)
- Konec --> ENDRX v DBGU_SR (Status register)
#### 2. Pisanje niza
- Zacetni naslov niza --> TPR register 
- Stevilo znakov niza --> TCR register 
- Zacetek DMA (oddajanje) --> TXTEN (enable, bit 8) v PTCR register
- Konec --> ENDTX v DBGU_SR registru

#### Addresses
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
      
      .equ DBGU_RPR,  0x100 	    /* Receive Pointer Register */
      .equ DBGU_RCR,  0x104    	  /* Receive Counter Register */
      .equ DBGU_TPR,  0x108       /* Transmit Pointer Register */
      .equ DBGU_TCR,  0x10C 	    /* Transmit Counter Register */
      .equ DBGU_RNPR, 0x110 	    /* Receive Next Pointer Register */
      .equ DBGU_RNCR, 0x114 	    /* Receive Next Counter Register */
      .equ DBGU_TNPR, 0x118 	    /* Transmit Next Pointer Register */
      .equ DBGU_TNCR, 0x11C 	    /* Transmit Next Counter Register */
      .equ DBGU_PTCR, 0x120 	    /* Periph. Transfer Control Register */
      .equ DBGU_PTSR, 0x124 	    /* Periph. Transfer Status Register */
```
##### MAIN
```
bl DEBUG_INIT

LOOP:
/* sprejemanje preko DMA - shranjevanje v niz1 */
      ldr r0, =niz1
      ldr r1, =STRING_LENGTH
      bl RCV_DMA

/* oddajanje preko DMA - branje iz niz2 */
      ldr r0, =niz2
      ldr r1, =STRING_LENGTH
      bl SND_DMA
      
/* cakanje na konec oddajanja - ENDTX */
      ldr r0, =DBGU_BASE
z1:   ldr r1, [r0, #DBGU_SR]
      tst r1, #1 << 4
      beq z1
      
/* cakanje na konec prejemanja - ENDRX */
      ldr r0, =DBGU_BASE
z2:   ldr r1, [r0, #DBGU_SR]
      tst r1, #1 << 3
      beq z2
      
/* zamenjava velikosti crk */
      ldr r0, =niz1
      ldr r1, =niz2
      ldr r2, =STRING_LENGTH
      bl CHANGE
      
      b LOOP
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
##### SND_DMA
```
SND_DMA:
      stmfd r13!, {r2,r3, lr}
      
      @ INIT
      ldr r2, =DBGU_BASE
      add r0, r0, #0x200000       /* Program in spremenljivke na naslovu 0x200000! */
                                  /* Program v RAM pomnilniku - kopija (zacetek v 0) */
      str r0, [r2, #DBGU_TPR]     /* Receive Pointer register (r0 = pointer na niz1) */
      str r1, [r2, #DBGU_TCR]     /* Receive Counter register (r1 = stevilo znakov) */
      mov r3, #1 << 8
      str r3, [r2, #DBGU_PTCR]    /* Periph. Transfer Control Register (RXTEN - bit 0) */

      ldmfd r13!, {r1-r2, pc}
```
##### RCV_DMA
```
RCV_DMA:
      stmfd r13!, {r2,r3, lr}
      
      @ INIT
      ldr r2, =DBGU_BASE
      add r0, r0, #0x200000
      str r0, [r2, #DBGU_RPR]     /* Receive Pointer register (r0 = pointer na niz1) */
      
      @ STRING_LENGTH med 0 in 80 
      cmp r1, #1     @ min = 1
      movlo r1, #1
      cmp r1, #80    @ max = 80
      movhi r1, #80
      str r1, [r2, #DBGU_RCR]     /* Receive Counter register (r1 = stevilo znakov) */
      mov r3, #1
      str r3, [r2, #DBGU_PTCR]    /* Periph. Transfer Control Register (RXTEN - bit 0) */

      ldmfd r13!, {r2-r3, pc}
```
##### CHANGE
```
CHANGE:
      stmfd r13!, {r3-r4, lr}
      
z3:   ldrb r3, [r0], #1
      mov r4, r3
      orr r4, #1 << 5   @ 5 bit 1 -> mala crka
      cmp r4, #'a'      
      blo naprej
      cmp r4, #'z'
      bhi naprej
      @ je crka                  
      eor r3, #1 << 5   @ obrnemo 5 bit 
naprej:
      strb r3, [r1], #1 @ shrani v niz2 (r1)
      subs r2, r2, #1
      bne z3
      
      ldmfd r13!, {r3-r4, pc}
```
##### Constatnts
```
/* variables */  
  
      .equ STRING_LENGTH, 10
      
niz1: .asciz  "__________"
niz2: .asciz  "abcdefghij"
      
      .align
```