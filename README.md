# Veckoschema

Ett veckoschema för barn — vem lämnar, vem hämtar, när det är fritids.
Gjort för att se bra ut på skärm, på surfplatta och utskrivet på ett A4 i liggande format.

## Använda det

Dubbelklicka på `index.html`. Det är hela appen — en enda fil, inget att installera,
fungerar utan internet.

- **Klicka på ett namn** för att byta person (Mamma → Pappa → tomt → Mamma …)
- **Klicka på Ja/Nej** för att växla morgonfritids och fritids
- **Klicka på tiden** för att skriva en ny tid
- **Klicka på veckonumret eller året** för att ändra vecka
- **Pilarna ‹ ›** hoppar en vecka fram eller tillbaka
- **Klicka på rubriken** för att byta namn på veckan
- **Två noteringsrader per dag** — klicka där det står "+ notering"

Datumen under varje veckodag räknas ut automatiskt från vecka och år, enligt svensk
veckonumrering (ISO). Årsskiften sköter sig själva: vecka 53 2026 följs av vecka 1 2027.

## Planera framåt

Varje vecka har sitt eget innehåll. Bläddrar du fram med **›** eller skriver in ett
veckonummer kommer du till en **tom** vecka som du kan fylla i i förväg — till exempel
att farfar hämtar på fredag nästa vecka.

När du inte står på innevarande vecka blir veckorutan markerad, och det står hur långt
bort du är ("nästa vecka", "om 2 veckor"). Knappen **Denna vecka** tar dig tillbaka.

På en tom vecka finns också **Kopiera förra veckan**, om rutinen är ungefär densamma
och du bara vill ändra en detalj.

Veckor du inte rört tar ingen plats, och tömmer du en vecka försvinner den igen.
Vilken vecka du tittar på är personligt — bläddrar du framåt flyttas inte den andras vy.

Allt sparas automatiskt i webbläsaren på den enhet du använder. Inget skickas någonstans.

Under **Inställningar** byter du rubrik, och lägger till eller byter namn och färg på
personerna.

## Skriva ut

**Skriv ut** ger ett A4 i liggande format med färgerna kvar. Knappar och
"+ notering"-texter försvinner. Skriver du ut från mobilen får du ändå hela
veckorutnätet, inte dagkorten.

## Dela

Det finns två sätt. Båda kräver att sidan ligger på en webbadress — öppnar du filen
lokalt blir länken en `file:///`-adress som bara fungerar på din dator.

### Gemensamt schema (samtidig redigering)

Ni redigerar **samma** vecka och ser varandras ändringar inom några sekunder. Den
andra personen behöver inget konto och ingen app, bara länken.

Kräver en engångsuppsättning: en gratis databas och en webbadress.
**Stegen står i [SETUP.md](SETUP.md).** När det är klart:
*Inställningar → Skapa gemensamt schema → Kopiera* och skicka länken.

I verktygsraden står det **Synkat** när allt är i ordning. Tappar du nätet står det
"Ingen kontakt – sparas lokalt"; ändringarna skickas upp av sig själv när
uppkopplingen är tillbaka. Medan du har markören i en ruta hämtas inga ändringar ner,
så du blir inte avbruten mitt i en mening. Skriver ni i samma ruta samtidigt vinner
den som sparar sist.

### Ögonblicksbild (utan uppsättning)

Utan databasen lägger **Dela**-knappen hela veckan i själva länken. Den som får den ser
exakt din vecka och kan ändra i sin egen kopia, men ni ser inte varandras ändringar —
du får skicka en ny länk när något ändras.

Schemat ligger i länken, inte på den publicerade sidan. Den som bara hittar
webbadressen ser ett tomt exempelschema, inte er vecka.

## På surfplattan

Öppna webbadressen och spara sidan som en genväg på hemskärmen. Sparade ändringar
ligger lokalt per webbläsare — ändrar du på datorn syns det inte automatiskt på
plattan förrän du öppnar en delningslänk där.

## Bygga vidare

Allt ligger i `index.html`: CSS överst i `<style>`, all logik nederst i `<script>`.

| Vad | Var |
| --- | --- |
| Raderna i schemat (Morgonfritids, Lämna, …) | `ROWS` |
| Dagarna | `DAYS` |
| Färger att välja mellan | `TONES` |
| Innehållet som visas första gången | `defaultState()` |
| Veckonummer och datum | `mondayOfISOWeek`, `isoWeekOf` |
| Delningslänkar | `encodeState`, `decodeState`, `readLink` |
| Gemensamt schema | `sync`-objektet, och `SCHEMA_SYNC` högst upp i filen |
| Utskriftens storlekar | `@media print` i CSS |

Idéer att lägga till härnäst: flera barn, bläddra mellan sparade veckor, eller se vem
av er som gjorde den senaste ändringen.
