
Delay, frekvenca, PWM, ciklicne prekinitve (merjenje temperature)
Uporaba 16 bitnih registrov

##### 1. Power Managment Control (PMC_PCER) 
- Omogocimo urin signal
- bazni register (PMC_BASE)
- **TC0 (Timer counter 0) -> bit 17**
##### 2.  TC - Channel Mode Register (TC_CMR)
- bazni register (TC0_BASE)
###### 2.1 Izbira frekvence urinega signala
- **TCCLKS -> bit 0,1,2**
- TIMER_CLOCK5 - SLCK (Slow clock) <-- 100
- **TIMER_CLOCK4 - MCK/128 <-- 011**
###### 2.2 Mode
- **WAVE -> bit 15**
- Capture mode (merjenje - INPUT) -> WAVE = 0
- Wave mode (generiranje - OUPUT) -> WAVE = 1
###### 2.3 Wave
- wave select
- **WAVSEL -> bit 13, 14**
-  00 (0 do max)
- 10 (0 do registra RC) <-----
- 01 (0 do max, max do 0)
- 11 (0 do RC, RC do 0)
##### 3. TC Register C (TC_RC)
- Rc meja
- do ketere meje naj stevec steje (dolzina intervala)
- interval 0-65535 (2^16 -1) <- 16 bit registri
- **16 bitna vredsnot**
##### 4. TC - Channel Control Register (TC_CCR)
- omogocimo TC stevec
- uro (CLKEN -> 0 bit)
- stevec (TRIGGER - reset) (SWTRG -> 2 bit)

------------------------------------------
#### 5. TC - Counter Value (TC_CV)
- counter value 
- min: 0 --> max: 65535 (COVFS flag)
- min: 0 --> RC (CPCS flag)
##### 6. TC - Status register (TC_SR)
- statusni regsiter TC counterja 
- cakanje na dogodek 
- COVFS - overflow (TC_CV registra) pridemo do max
- CPCS - dosezemo RC vrednost (TC_CV register)
- CPCS -> 1 (1 perioda)
- Ko beremo z registorv se vrednsot nastavi na 0 (ponovno pristvanje - RESET)


#### Program 
- INIT_TC0 (inicializacija)
- DEALY_TC0
- 1Hz utripanje - 1 perioda T = 1000ms
- (DELAY: ON) + (DELAY: OFF)  = 1000ms
- DELAY: ON = 100
- DELAY: OFF = 900

##### Addresses
```
	  .equ PMC_BASE, 	0xFFFFFC00	/* Power Management Controller */
      .equ PMC_PCER, 	0x10  	  	/* Peripheral Clock Enable Register */
      .equ TC0_BASE, 	0xFFFA0000	/* TC0 Channel Base Address */
      .equ TC_CCR,  	0x00  		  /* TC0 Channel Control Register */
      .equ TC_CMR, 	  0x04	  	  /* TC0 Channel Mode Register*/
      .equ TC_CV,    	0x10	    	/* TC0 Counter Value */
      .equ TC_RA,    	0x14	    	/* TC0 Register A */
      .equ TC_RB,    	0x18	     	/* TC0 Register B */
      .equ TC_RC,    	0x1C	    	/* TC0 Register C */
      .equ TC_SR,    	0x20	    	/* TC0 Status Register */
      .equ TC_IER,   	0x24	    	/* TC0 Interrupt Enable Register*/
      .equ TC_IDR,   	0x28		    /* TC0 Interrupt Disable Register */
      .equ TC_IMR,  	0x2C	    	/* TC0 Interrupt Mask Register */
```
##### INIT_TC0
```
INIT_TC0: 
      stmfd r13!, {r0,r1, lr}
      
	  ldr r0, =PMC_BASE
	  mov r1, #1 << 17         
	  str r1, [r0, #PMC_PCER]	  /* Enable clock signal na TC0 */
      
      ldr r0, =TC0_BASE
      mov r1, #0b011              /* TCCLKS -> TIMER_CLOCK4 (MCK/128) */
      mov r1, #1 << 15            /* WAVE -> Wave mode (bit 15 = 1)   */      
      mov r1, #0b10 << 13         /* WAVSEL -> nacin (0 do RC)(bit 13,14 = 10) */
      
      mov r1, #0b110 << 13
      add r1, r1, #0b011
      str r1, [r0, #TC_CMR]       
      
      ldr r1, =375                /* meja RC registra 65535 = max */
      str r1, [r0, #TC_RC]        /* RC value = 375 */
      
      mov r1, #0b101              /* Enable TC (CLKEN = 1) bit0 */
      str r1, [r0, #TC_CCR]       /* Start TC  (SWTRG = 1) bit2 */
      
      ldmfd r13!, {r0,r1, p
```
##### DELAY_TC
```
DELAY_TC:
      stmfd r13!, {r1-r4, lr}
      sub r0, r0, #1    @ loop1: 499 -> 0 (500x)
      ldr r2, =TC0_BASE
      ldr r4, [r2, #TC_SR]        /* Counter reset */ 

TCloop1:
TCloop2:
      ldr r4, [r2, #TC_SR]        /* LOAD - Status registr */
      tst r4, #0b10000            /* test CPCS - bit4 (1 perioda) */
      beq TCloop2
      
      subs r0, r0, #1
      bne TCloop1
      
      ldmfd r13!, {r1-r4, pc}
```

#### Blinking LED
- hitrjese preklaplanje (manjsa perioda)
- LED dimming frequency (100Hz - 5kHz)
- F(1Hz) = T(1000ms) <-- 1 perioda
- 100Hz = T(10ms)
- Duty cycle (% periode, kjer je LED = ON)
- Duty = 90% (90% - ON, 10% - OFF)

##### F = 100Hz, duty = 80% (Bright)
- T = 10ms
- DELAY: ON = 8
- DELAY: OFF = 2

##### F = 100Hz, duty = 20% (Dark)
- T = 10ms
- DELAY: ON = 2
- DELAY: OFF = 8

```
      bl INIT_IO
      bl INIT_TC0
LOOP: bl LED_ON
      ldr r0, =2                  /* duty cycle 20% */
      bl DELAY_TC
      bl LED_OFF
      ldr r0, =8
      bl DELAY_TC
      
      b LOOP
```

##### PWM control - TODO