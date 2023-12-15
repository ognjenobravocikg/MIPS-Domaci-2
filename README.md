# MIPS-Domaci-2

ELEKTRONSKE KOMPONENTE I PODESAVANJA STM32CubeIDE <br> <br>
Za izradu domaceg zadatka sam koristio temperaturni senzor DS18B20, mikrokontroler STM32F103C6Tx, Pololu U3V50F5 step-up voltazne regulatore, drajvere za motor A4988, kao i step DC motor sa 1 stepenom po stepu.
1. DS18B20 ima 3 pina, jedan na koji ulazni napon 3.3 volta koji sluzi kao napajanje, jedan za ground, kao i Data pin preko kojeg saljemo procitanu temperaturu
2. STM32F103C6Tx  sam povezao na sledeci nacin
![image](https://github.com/ognjenobravocikg/MIPS-Domaci-2/assets/94371789/403572e3-ecfe-4ee4-be18-b5afca7a939c) <br>
   u RCC za HSE sam izabrao Ceramic/Crystal Resonator,  ukljucio sam unutar Connectivity tab-a USART1, a unutar Analog tab-a sam aktivirao ADC 1 sa standardnim parametrima sto nam obradjuje podatke sa senzora temperature, jos sam unutar Clock-a uradio sledece promene kako bih za ADC1 clock imao 12 MHz
   ![image](https://github.com/ognjenobravocikg/MIPS-Domaci-2/assets/94371789/eba9b3d8-d680-4dfd-a592-8b679843c456) <br>
3. Bucuci da kada aktiviramo output za neki PIN na STM32F103C6Tx on ce izbaciti 3.3 volta sto nije dovoljno za pokretanje mojih drivera A4988 (potrebno je 5 volta). Koristio sam Pololu U3V50F5 koji ce uzeti nasu 3.3 voltazu i pretvoriti je u 5 volta (mogu se koristiti i optokapleri)
4. Drajveri A4988 ima STP i DIR pinove, STP kontrolise da li se i koliko se brzo okrecu nasi motori, DIR pin kontrolise u kom smeru se okrecu nasi motori
5. Na kraju koristio sam step DC motore M1 i M2, koji se okrecu 1 stepen po pulsu.

KOD IZ STM32CubeIDE objasnjenje <br> <br>
Objasnjenje svih funkcija u fizickom svetu. 

Ovaj korak obavljam bezbroj puta, dok ne ubacim sve epruvete koje zelim na traku. 
![image](https://github.com/ognjenobravocikg/MIPS-Domaci-2/assets/94371789/9c466313-b53e-49ea-90ad-b49e896877b0) <br>
U ovom koraku citam temperaturu na senzoru koju su proizveli motori i pretvaram je u vrednost koju mogu ocitati pomocu neke integer vrednosti. Nakon tog koraka proveravam da li je temperatura 75 stepeni (koristio sam svoje ime kao Ognjen Obradovic kako bih imao 15 slova u imenu radi lakseg racuna) TEMP= N_s*5. Ako nije onda ucitavam neku vrednost sa tastature, u kodu sam je primenio kao slucajnu vrednost izmedju 0 i 4. Primenio sam sledecu funkciju kako bih proverio najblizu distancu do zeljene epruvete (unutar samog koda mozete videti celu funkciju)
![image](https://github.com/ognjenobravocikg/MIPS-Domaci-2/assets/94371789/2fcf584a-bfee-4fef-b48c-55891762ce8d) <br>
Iz ove funkcije vracam primera radi -3 vrednost, sto znaci da cu morati da se okrenem za 3 slova ulevo. 
Potom sam okrenuo motor 1 dok nisam stigao do zeljene epruvete, buduci da moje ime ima 15 slova to znaci da moram preci 360/15=24 stepena kako bih dosao do sledeceg slova, a buduci da se moj motor okrece jedan stepen po pulsu to znaci da cu morati da saljem jedinicu na adekvatan output 24/1=24 puta.
![image](https://github.com/ognjenobravocikg/MIPS-Domaci-2/assets/94371789/5f980e2e-52d2-47d6-9817-f9bc44a92395) <br>
Na ovaj nacin sam stigao do zeljene epruvete, nakon koje se moze implementirati neka mehanicka ruka koja ce je izbaciti na nasu pokretnu traku. Potom sam isto pokretnu traku pomerio 1 od 15 zljebova, kako bi mogla da se izgura sledeca epruveta. Dati proces sam ponavljao sve dok nisam ocitao vrednost preko 75 stepeni, nakon koje sam pokretnu traku pomerio 15 zljebova i na taj nacin sve epruvete ubacio u neki kontejner, ili slicno. 

Prilikom primene ovog koda bitno je staviti senzor temperature blizu dva motora koji obavljaju ove radnje. 


