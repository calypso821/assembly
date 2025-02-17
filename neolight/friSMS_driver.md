Kontrola NeoPixel LED traku s counterjem - TC (FRI-SMS)
Stvar naceloma deluje, s TC counterjem generiram 0 in 1. 

V labaratoriju sem pogledal izhodni signal in ugotovil, da je problem pri prehodu iz 0 v 1 ali 1 v 0.
Resitev: counter bi moral po vsaki novi napolniti RA registra (dolzina high stanja) resetirat na 0, tako bi bila uporabljena nova vrednsot. 

**[Driver code](friSMS_driver_code.md)**

![image](images/FRI_SMS_PWM.png)
![image](images/NeoPixel_FRI_SMS.png)

