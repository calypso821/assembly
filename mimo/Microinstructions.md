
Jasno zapišite **zaporedje mikroukazov**, ki se izvedejo pri izvedbi strojnega ukaza "**LI Rd,Immed**" (primer: "LI   r3, 150").
- ta ukaz je že implementiran v datoteki z definicijami mikroprogramskih realizacij
- ob vsakem mikroukazu krajše **opišite njegovo delovanje** (stanje podatkovne enote, kaj se pravzaprav takrat zgodi, itd....)

```
# LI Rd, Immed
fetch:	addrsel=pc irload=1 					              # Address=PC, Load IR register
		pcload=1  pcsel=pc, opcode_jump			              # PC=PC+1, jump to 2+OPC
63:	    addrsel=pc  dwrite=1  regsrc=databus, goto pcincr	  # immed read, then pcincr
pcincr:	pcload=1  pcsel=pc, goto fetch                        # PC=PC+1, jump to fetch
```

##### fetch:	addrsel=pc irload=1 
- addrsel=pc (nastavitev naslova RAM, kamor kaze PC)
- irload=1 (prebran ukaz nalozimo v ukazni (instruction) register)

##### pcload=1  pcsel=pc, opcode_jump
- pcload=1 (aktivirano povecanje programskega stevca)
- pcsel=pc (izberemo nacin povecevanja PC --> pc = pc+1)
- opcode_jump (skok na naslov OP_code ukaza +2)

##### 63:	    addrsel=pc  dwrite=1  regsrc=databus, goto pcincr
- addrsel=pc (nastavitev naslova RAM, kamor kaze PC)
- dwrite=1 (pisanje v d-register)
- regsrc=databus (izvor nastavljen na data linijo) - naslov kamor kaze PC v RAM
- goto pcincr (skos na pcincr naslov)

##### pcincr:	pcload=1  pcsel=pc, goto fetch  
 - pcload=1 (aktivirano povecanje programskega stevca)
 - pcsel=pc (izberemo nacin povecevanja PC --> pc = pc+1)
 - goto fetch (skok na zacetek 0 - branje novega ukaza na PC+1)


