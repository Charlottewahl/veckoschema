# Veckoschema

Ett veckoschema för barn — vem lämnar, vem hämtar, när det är fritids.
Gjort för att se bra ut på skärm, på surfplatta och utskrivet på ett A4 i liggande format.

## Använda det

Dubbelklicka på `index.html`. Det är hela appen — en enda fil, inget att installera,
fungerar utan internet.

- **Klicka på ett namn** för att byta person (Mamma → Pappa → tomt → Mamma …)
- **Klicka på fritidsrutan** för att växla tomt → Ja → Nej → tomt. En vecka ingen rört
  står tom, så den inte ser ut att påstå "nej" till fritids
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

**Rensa veckan** tömmer hela den vecka du står på, efter en bekräftelsefråga.

## Lediga dagar

Varje dag har en bockruta **Ledig**. Bockar du i den försvinner lämning, hämtning,
morgonfritids och fritids för den dagen, och i liggande läge ersätts hela dagens
kolumn av ett stort **Ledig**-block.

Noteringarna står kvar, så du kan skriva dit vad som gäller istället — "hos farmor",
"åker till stugan".

Observera att i-bockningen **raderar** dagens tider. Bockar du ur rutan igen kommer de
inte tillbaka, dagen blir tom.

Veckor du inte rört tar ingen plats, och tömmer du en vecka försvinner den igen.
Vilken vecka du tittar på är personligt — bläddrar du framåt flyttas inte den andras vy.

## Vem ändrade, och när

Under schemat står en liten rad: *Ändrad idag 17:20 av C*. Initialerna anger du under
**Inställningar → Den här enheten**. De sparas bara på din enhet, medan själva stämpeln
följer med schemat, så den andra ser vem som rört veckan. Lämnar du fältet tomt visas
bara tidpunkten.

Stämpeln gäller per vecka, och den följer med på utskriften så man ser hur färsk
en utskriven lapp är.

## På skärm och på papper

Stående läge, på mobil och platta, visar ett kompakt kort per dag — det är läget som
är gjort för att överblicka. Liggande läge med hela veckorutnätet används på större
skärmar och är det som skrivs ut, oavsett vilken enhet du skriver ut från.

Allt sparas automatiskt i webbläsaren på den enhet du använder. Inget skickas någonstans.

Under **Inställningar** byter du rubrik, och lägger till eller byter namn och färg på
personerna.

## Skriva ut

**Skriv ut** ger ett A4 i liggande format med färgerna kvar. Knappar och
"+ notering"-texter försvinner. Skriver du ut från mobilen får du ändå hela
veckorutnätet, inte dagkorten.

## Skicka som bild

**Bild** gör veckan till en PNG i liggande format och i färg — även när du står i
stående läge på en telefon.

På mobil öppnas delningsrutan direkt, så bilden går att skicka i vilken app som helst.
På dator laddas den ner som `veckoschema-v46-2026.png`.

Bilden ritas upp från grunden på en canvas, inte som en skärmdump av sidan. Därför ser
den likadan ut i alla webbläsare, och den landar på ungefär 180 kB — liten nog att
skicka i en chatt. Den visar den vecka du står på, med lediga dagar och noteringar.

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
