- Grobo zrnatost (preklop niti, ko pride dod daljsega cakanja) - enostavnejsi, manj preklopov boljse pri manjsem\ stevilu niti

- Cevovovd - paralizem na nivoju ukazov (transparentsnot)
- Sestavljen iz vec stopenj (v vsaki urini periodi star ukaz izstopi in nov vstopi v stpnjo - paraleleno izvajanje)
- Prevezem ukaza (Instruction Fetch)
- Dekodiranje ukaza (Instrction Decode)
- Execute (Izvrstiev - ALU)
- Memory accesss (dostop do polnilnika LDR, STR)
- Write back (shranjevanje v registre)
- 5 stopenj - 5 ukazov paralelno
- n-kratno povecanje dostopa do polnilnika 
- optimalno stevilo cevovodnih stopenj (12-20) zaradi cevovodnih nevarnosti

- Cevovodne nevarnosti
- Strukturne (registri, ALE, predpomnilnik...)  - te nevarnosti se najmanj skodljive
- locn predponilnik za ukaze in operande (Hardwadrska arhitektura)
- Vecje stevilo FE enot (2x ALE...)
- Stponja Execute v obliki cevovoda 

- Podatkovne (odvisnost operandov, ukaz potrebuje operand, ki se ni na voljo) ~ 5% povecanje CPI
- RAW (read after write) - najbolj nevarno (operand, ki ga beremo se ni na voljo) branje - stopnja 2, pisanje stopnja 5
- WAR, WAW - pisanje v 5 stopnji, branje v 2 stopnji - OK
- Resitve (**cevovodno razvrscanje** - prevajalnik (NOP operacije), zaklenitev cevovda (HW - Nop), premoscanje - rezultati na voljo ze pred shranjevnajem (povezava iz execute v devode)

- Kontrolne nevarnosti (skoki) ~ 21% povecanje CPI
- Napoved neizpolnjenega skoka (polnimo ukazi, ki so za ukazom, ce se zgodi skosk (napacna napoved), pocitimo cevovod - razveljavimo ukaze)
- Slaba resitev - NOP operacije
- Preverjanej pogoja in izracun skocnega naslova cim bolj na zacetku (manj ukazov, ki jih moramo zavreci)
- Predikcije (staticne, dinamicne)

- Staticna - pogoj neizpolnjenega pogoja (pogoj izpolnjenga - TEZJE - potrebujemo se skocni naslov)
- Zakasnjenimi skoki (skocne reze - takoj za skocnim ukazom) dodama ukaze, ki ne vplivajo na izid skok, drugace NOP
- Preprostost, ucinkovitsot pri krajsih cevovodih 

- Dinamicnmo - napoved se spreminja med delovanjem 
- 1 bitna predikstorska tabela (hrani zgodovilo zadnjega izida skoka) hrani naslov in izid skoka
- Pravilna napoved - ON, napcna napoved - pocistimo cevovod (razveljavimo ukaze), popravimo tabelo izidov 
- 2 bitna predikcija - hrani zgodovino zadnjih 2 izidov (0,1 - izpolnjen pogoj, 2,3 - neizpolnjen pogoj)
- 2x zaporedna zgresitev - se napoed spremeni
- Korelacijski predikator (tabelo razdelimo na vec 2-bitnih prediktorskih tabel) - glede na izpolnjenost zadnjih 2 napovedi se izbere ena izme 0,1,2,3 tabel
- informacija o lokalnem skocenm ukazu (n-bitna prediskotrska tabela)
- informacija o globalnem obnasanje zadnjih n - ukazov\
- Turnirsi preditkor (Globlani prediktor - (2,2) + Lokalni selektor (0,2) + selektor) - zdruzevanje napovedi razlicnih prediktorjev
- Aktulani gshare, TAGE, perceptron

- Kompleksnjesi ukazi, ki trajajo vec urnih period (Execute stopnja) -> zgradimo vecperiodne Funkcijske enote (ALU...)
- Strukturne nevarnosti (ukaz v enoti x urnih period - drugi ukazi za to enoto cakajo)
- Strukturne nevarnosti odpravimo s cevovodno realizacijo Execute stopnje (vmesni rezlutati)
- Podatkovne nevranost znotraj execute (premoscanje ni mozno) - dinamicno razvrscanje (CPE spremeni vrstni red izvajnja ukazov)

- Branje operadnov (preverjanje podatkovnih nevarnsoti) - out of order
- Execute - izvrsevanje ukazov - out of order 
- Tomasulov algoritev (preimenovanje registrov, porazdeljena logika (drugacen - out of order vrstni red), boljsa ucinkovitost pri zgresitvah v predpomnilniku (izvajanje drugih ukazov)
- Dodamo Rezervacisjke postaje (cakanje ukazov za operande za izvrsitev ) za vsako Funkcijsko enoto 
- Izvrsevanje ukazov v drugacnem vrstnem redu - kontrolne nevarnosti, nimamo mehanizma za razveljavitev ukazov -> resitev spekulativno izvajanje

- Spekulativno izvajanje - omogoca pogojno izvajnaje ukazov (razveljavitev ce je napacna napoved)
- PI - preureditveni izravnalnik (reorder buffer) - FIFO vrsta (skrbi za vrstni red)
- Zacasna shramba, lahko zavrzmo ukaze ce napoved ni pravilna
- Ko se napoved potrdi -> gre ukaz v WB stopnjo (enka vrstni red)
- ROB - reoder buffer (FIFO vmesnik, ki vsebuje ukaze (operande in rezlutate), dokler ni potrjena pravilnost) - novo, staro stanje
- Ukazno vodilo (Cakalna vrsta ukazov -> ROB + Rezervacijske postaje)
- Podatkovno vodilo - premoscanje FE enote -> Rezervacijske postaje
- Operandno vodilo -

- Preureditveni izravnlanik porabi najvec energije (27%) - resitev eklsplicitno preimenovanje registrov
- razsirjena mnozica regisotrv, za zacasne rezultate
- preimenovalna tabela (zasedeni, prosti, uporabnikski)
- Reorder buffer se poenostavi (ne hranimo vec rezultatov), zagotavlja le se enakt vrsnit red ukazov
- Odpravimo podatkovne nevranosti WAW, WAR

- Strukturne nevarnosti (cakanje na prosto FE enoto) -> cevovovdne EX enota 
- Podatkovne neversnoti (cakanje operandov) -> Tomasulov algoritem 
- Kontrolne nevarsnoti (Razveljavitev ukazov) -> spekulativno izvajanje

- Dinamicna predikcija skokov (napved skoska hitrejsi + pocasnjesi)
- Dinamicno razvrscanje ukaov (CPE spremeni vrstni red izvajanja - rezervacijske reze)
- Spekulativno izvrsevanje ukazov (razveljavitev ukazov, Reorder buffer)
- Preimenovanje regisstrov

CPI << 1 ?? vec ukavoz hkrati - vecizstavitveni procesor
Prevzem n - ukzov (ukazni predpomnilnik n - ukazov)
Preverjanje medsebojne odvisnosti operandov (SW, HW)
- SW prevajalniki (Very long instruction word)
- HW dinamicno med izvajanjem (supersklarni procesorji) n^2 -n primerjav
- SUperskalarni (vecje stevilo funckijskih enot in vecji preureditveni izravnalnik)
- Very long instrucution word - dolgi ukazi (ukaz za vsako funkcijsko enoto), ce ne najde ukazov vstavi NOP
- za specificne namene (digitalno procesiranje signalov) iste opracije

Najvecja nevarsnot je zgresitev v predpomnilnikih (cakanje tudi do 130 urnih period)
Vec nitnost (nekaj malega vec logike, veliko ceja zmogljivosti) F
- FE, predpolminlniki -> skupni
- ROB, registerske tabele -> loceni
- cevovovd - 2 toka ukazov (boljsa izkoriscenost, ko cakamo na operande)
- Ni vec transparentno - programer skrbi za vec niti
- Drobnozrnatost (preklapljanje niti vsako urino periodo) - zapletena implementacija (shranjevanje cevovoda)
- Grobo zrnatost (preklop niti, ko pride dod daljsega cakanja) - enostavnejsi, manj preklopov boljse pri manjsem stevilu niti
- Istocasna vecnitnost (SMT) (vec izstavitevni - kjer lahko izvajamo vec ukazov vzporedno) izvajanje vec niti vzporedno
- Dobro - ce je malo zgresitev v predpomnliniku  (locen predpominlnik)
- Slabo - ce imamo veliko zgresitev (skupen predpomnilnik) v predpomnilniku
- Instructuion in Data cache - skupen (niti si ga delijo) - vecja moznost za zgresitev predpomnilnika
- Fiskno razdeljeni, kdor prvi pride prvi melje, 

- Hyperthreading i7 - istocana vecnitnot (manj porabva cirov, kot je pohitritev +)

- Vec jeder (paralizem na nivojev procesorjev - ni traspranentno, programerjeva naloga)
- Homogeni multiprocesorji na cipu (vec eenakih CPU jeder, ki si delijo glavni pomnilnik)
- SISD (single instruction, Single data) - Von neumann model
- SIMD (single instrction, Multiple data) - paralziem na nivoju podatkov
- MIMD (Multiple instruction, Multiple data) - splsnejsi od SIMD, vec paralizma

- Multiprocesorji - skuoni naslovni prostor (pominlnik)
- Multiracunlaniki - loceni pominlnik (cenje, tezje progrmiarnje)

