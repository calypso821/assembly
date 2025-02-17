
PCK -> Main Clock (ura) = 192 MHz
MCK -> (V/I clock) = PCK/4 = 48MHz
Enable caches


PIO - Parallel INPUT/OUTPUT
Vrata A,B,C (0-31 pinov)
- PA{0-31}
- PB{0-31}
- PC{0-31}

V/I krmilimo z registri V/I naprave!
1. Locen V/I naslovni prostor (posebni ukazi) ldio, stio...
2. Pomnilnisko preslika V/I (del pomnilnika namenjen registor V/I naprave) - normalni ukazi

LED = PC1 (bitx = 1)

----------- LED INIT --------------
##### 1. PIO_PSR (PIO status register)
- bitx = 1 (PIO krmilnik - digitalni vhod/izhod)
- bitx = 0 (druga V/I naprava) - uart transmit... 
- PIO_PER (write only) - ENABLE (bit set)
- PIO_PDR (write only) - DISABLE (bit clear)
##### 2. PIO_OSR (Output status register)
- bitx = 1 (Izhod)
- bitx = 0 (Vhod)
- PIO_OER (Enable)
- PIO_ODR (Disable)

---------- LED kontrola -------------
##### 3. PIO_ODSR (Output data status register)
- stanje izhoda
- bitx = 1 (High) - LED OFF
- bitx = 0 (Low) - LED ON
- PIO_SODR (Set)
- PIO_CODR (Clear)

```
/* user code here */
			.equ PIOA_BASE, 0xFFFFF400 /* Zacetek registrov za vrata A - PIOA */
			.equ PIOB_BASE, 0xFFFFF600 /* Zacetek registrov za vrata B - PIOB */
			.equ PIOC_BASE, 0xFFFFF800 /* Zacetek registrov za vrata C - PIOC */

			.equ PIO_PER, 0x00	/* Odmiki... */
			.equ PIO_OER, 0x10
			.equ PIO_SODR, 0x30
			.equ PIO_CODR, 0x34

			ldr r0, =PIOC_BASE
			mov r1, #1 << 1         /* lsl #1 */
			str r1, [r0, #PIO_PER]	/* Prikljucek C1 krmili PIO */
			str r1, [r0, #PIO_OER]	/* Omogoci izhod na C1 */

			@str r1, [r0, #PIO_SODR]	/* Na prikljucek C1 zapiši stanje 1  - LED (OFF) */
			str r1, [r0, #PIO_CODR]	/* Na prikljucek C1 zapiši stanje 0  - LED (ON) */

/* end user code */
```
##### Addresses
```
      .equ PIOC_BASE, 0xFFFFF800 /* Zacetek registrov za vrata C - PIOC */
      .equ PIO_PER, 0x00	/* Odmiki... */
      .equ PIO_OER, 0x10
      .equ PIO_CODR, 0x34
      .equ PIO_SODR, 0x30
```
##### MAIN
```
/* user code here */
      bl INIT_IO
LOOP: bl LED_ON
      ldr r0, =200
      bl DELAY
      bl LED_OFF
      ldr r0, =200
      bl DELAY
      
      b LOOP
/* end user code */
```
##### INIT_IO
```
INIT_IO: 
      stmfd r13!, {r0,r1, lr}
	  ldr r0, =PIOC_BASE
	  mov r1, #1 << 1         /* lsl #1 */
	  str r1, [r0, #PIO_PER]	/* Prikljucek C1 krmili PIO */
	  str r1, [r0, #PIO_OER]	/* Omogoci izhod na C1 */
       
      ldmfd r13!, {r0,r1, pc}
```
##### LED_ON
```
LED_ON:  
      stmfd r13!, {r0,r1, lr} 
      ldr r0, =PIOC_BASE
	  mov r1, #1 << 1 
      str r1, [r0, #PIO_CODR]	/* Na prikljucek C1 zapiši stanje 0  - LED (ON) */ 
      ldmfd r13!, {r0,r1, pc} 
```
##### LED_OFF
```
LED_OFF: 
      stmfd r13!, {r0,r1, lr}   
      ldr r0, =PIOC_BASE
      mov r1, #1 << 1 
      str r1, [r0, #PIO_SODR]	/* Na prikljucek C1 zapiši stanje 1  - LED (OFF) */ 
      ldmfd r13!, {r0,r1, pc}
```
##### DELAY
```
DELAY:
      stmfd r13!, {r1, lr}
      sub r0, r0, #1    @ loop1: 499 -> 0 (500x)
loop1:ldr r1, =48000    @ loop2 ~ 1ms
loop2:subs r1, r1, #1
      bne loop2
      
      subs r0, r0, #1
      bne loop1
      ldmfd r13!, {r1, pc}
```