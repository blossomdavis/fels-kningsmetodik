# Felsökningsmetodik - guide

1. **Identifiera problemet:** Samla in fakta.
Den viktigaste frågan i detta skede är: *"Vad har ändrats nyligen?"* (Uppdateringar, nätverksändringar, nya certifikat?).
*Verification:* Du har en tydlig problembeskrivning, vet exakt vad som inte fungerar och hur det avviker från normalt beteende. 

tips: försök att återskapa händelsen, om det går. 


2. **Upprätta en teori om trolig orsak:** Börja med det enklaste.
Använd din kunskap om miljön för att lista möjliga orsaker. Börja alltid nerifrån och upp i OSI-modellen (fysiska kablar och ström först). Lista teorierna från mest sannolik till minst sannolik. Ta även hänsyn till kända buggar eller larm i ert övervakningssystem.


3. **Testa teorin:** Isolera felet.
Testa din mest sannolika teori. Om teorin inte stämmer, förkasta den och gå vidare till nästa på listan. Om testet kräver systemändringar som påverkar produktion, säkerställ att du vet hur du återställer ändringen innan du utför den.
*Verification:* Du har bekräftat exakt vilken komponent, tjänst eller konfiguration som orsakar avbrottet.


4. **Skapa en åtgärdsplan och implementera lösningen:** Minimera risk.
Skapa en steg-för-steg-plan för att åtgärda problemet. Vid kritiska system: boka ett servicefönster om möjligt och ha *alltid* en rollback-plan redo om din fix gör saken värre. Genomför sedan ändringen.


5. **Verifiera full systemfunktionalitet:** Testa hela kedjan.
Kontrollera att problemet är löst, men verifiera också att inga andra beroende system har gått sönder av din lösning. Om möjligt, låt användaren som anmälde felet bekräfta att det fungerar från deras sida.
*Verification:* Systemloggarna är rena från felmeddelandet och tjänsten svarar normalt på anrop.


6. **Dokumentera allt:** För framtiden.
Skriv ner symptom, den faktiska grundorsaken (Root Cause), den exakta lösningen och vilka system som påverkades i ert ärendehanteringssystem eller i er interna wiki.
*Verification:* En kollega ska kunna läsa dina anteckningar och lösa exakt samma problem om det uppstår inatt klockan 03:00.


### Gyllene regler för IT-infrastruktur (Pro-tips)

* **Ändra bara en sak i taget:** Om du gör tre konfigurationsändringar samtidigt och systemet plötsligt hoppar igång, vet du inte vilken av ändringarna som löste problemet (eller om de andra två skapade dolda problem).
* **Trust, but verify:** Användare och applikationsutvecklare menar väl, men deras felbeskrivningar ("Nätverket är nere") betyder ofta något helt annat ("DNS svarar inte" eller "Applikationspoolen har stannat"). Verifiera alltid felet med egna ögon och via loggar.
* **Lita på OSI-modellen:** Felsök systematiskt. Kolla Lager 1 (fysisk länk/ström) och Lager 2 (MAC/VLAN) innan du börjar rota i komplexa routingprotokoll (Lager 3) eller applikationscertifikat (Lager 7).
* **Följ trafikflödet:** Vid nätverks- eller kommunikationsproblem, följ paketet från källan till destinationen steg för steg (Klient -> Switch -> Brandvägg -> Load Balancer -> Server).
* **Det är (nästan) alltid DNS eller certifikat:** Om ett internt system som fungerade igår plötsligt slutar prata med en annan server idag, och inga uppdateringar har gjorts – dubbelkolla omedelbart om ett SSL-certifikat har gått ut eller om namnsupplösningen pekar fel.