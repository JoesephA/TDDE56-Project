# Metoder och upplägg – vår gemensamma förståelse

Det här dokumentet förklarar *hur* vi tänker angripa projektet, så att alla i gruppen har samma bild
innan vi börjar koda. Det är ett levande dokument – ändra och lägg till när vi bestämmer saker.

> **Status:** diskussionsunderlag. Inget är beslutat än. Öppna frågor finns längst ner.

---

## 1. Problemet i en mening

Givet historiska veckovärden av värmeeffekt (kW) för 70 byggnader i Linköping: förutsäg de
**nästa H veckorna** (H ≥ 4), och undersök hur bra det går när H växer.

## 2. Vad vi vet om datan

| Egenskap | Värde |
|---|---|
| Upplösning | 1 värde per vecka (medeleffekt i kW) |
| Längd | ca 2 140 veckor ≈ 41 år |
| Antal serier | 1 |
| Förklarande variabler | inga (ingen temperatur, inget väder) |

Vad vi *förväntar* oss att se (ska verifieras i EDA):

- **Stark årssäsong** – högt på vintern, lågt på sommaren. En säsong = ~52 veckor.
- **Långsiktig trend** – troligen nedåtgående (varmare klimat, bättre isolering) men kan också påverkas av att byggnadsbeståndet förändras.
- **Brus från vädret** – en kall vecka i februari går inte att förutsäga månader i förväg.

## 3. Grundbegrepp (kort ordlista)

- **Tidsserie** – mätvärden i tidsordning. Ordningen spelar roll, till skillnad från "vanlig" tabelldata.
- **Horisont (H)** – hur många steg (veckor) framåt vi förutsäger.
- **Fönster / lags (W)** – hur många tidigare veckor modellen får se som input.
- **Supervised-format** – vi gör om serien till en tabell: varje rad har W gamla värden som X och de H nästa som y. Funktionen `create_supervised` i notebooken gör just detta.
- **Säsong** – mönster som upprepas med fast period (här 52 veckor).
- **Stationär** – seriens statistiska egenskaper (medel, varians) ändras inte över tid. Många klassiska modeller kräver det; man uppnår det ofta genom *differentiering* (ta skillnaden mellan på varandra följande värden).
- **Baslinje** – en enkel referensmodell. En avancerad modell som inte slår baslinjen är inte värd något.
- **Dataläcka (leakage)** – när information från framtiden råkar komma in i träningen. Det största felet man kan göra med tidsserier.

## 4. Uppgifterna från projektbeskrivningen

1. Förstå notebooken och datan.
2. Jämför olika ML-modeller – med *korrekt* tidsserieutvärdering (Cerqueira et al., 2020).
3. Jämför prognosstrategier för flerstegsprognoser (Taieb et al., 2012).
4. Studera hur noggrannheten påverkas av horisonten.

## 5. Steg för steg – förslag

### Steg 1: Utforskande analys (EDA)
- Plotta hela serien och några enskilda år ovanpå varandra.
- **Dekomposition** (t.ex. STL) → dela upp i trend + säsong + rest.
- **ACF/PACF** (autokorrelation) → hur mycket beror en vecka på förra veckan, och på samma vecka förra året (lag 52)?
- Ändras säsongsmönstret över decennierna? (intressant för klimatdiskussionen)

### Steg 2: Baslinjer
| Modell | Idé |
|---|---|
| Naive | Nästa vecka = senaste veckan |
| Seasonal naive | Vecka *t* = samma vecka förra året |
| Klimatologi | Medelvärdet för det veckonumret över alla tidigare år |
| ETS / Holt-Winters | Exponentiell utjämning med trend + säsong |
| SARIMA / ARIMA + Fourier | Klassisk statistisk tidsseriemodell. SARIMA med period 52 är tung, så Fourier-termer (sin/cos) för säsongen är ett vanligt alternativ |

### Steg 3: ML-modeller
Alla tränas på supervised-formatet. Möjliga features: lags, sin/cos av veckonummer, rullande medel, värdet för 52 veckor sedan.

| Familj | Exempel | Kommentar |
|---|---|---|
| Linjära | Linear regression, Ridge, Lasso | Snabba, tolkningsbara, ofta starka på små data |
| Trädbaserade | Random Forest, Gradient Boosting, LightGBM/XGBoost | Kan inte extrapolera en trend – kan kräva detrending |
| Neurala nät | MLP (finns i notebooken), ev. LSTM/GRU | Lite data → risk för överanpassning. Intressant att se om de ens slår Ridge |

### Steg 4: Prognosstrategier (Taieb et al., 2012)
Hur förutsäger man flera steg framåt?

| Strategi | Hur | För- / nackdel |
|---|---|---|
| **Rekursiv** | En 1-stegsmodell, matar in sin egen prediktion som input för nästa steg | Enkel; fel ackumuleras |
| **Direkt** | En separat modell för varje h = 1…H | Ingen felackumulering; H modeller att träna, stegen "vet inget" om varandra |
| **DirRec** | Kombination: separata modeller som också får tidigare prediktioner som input | Mer komplex |
| **MIMO** | En modell som ger hela vektorn med H värden direkt | Fångar beroenden mellan stegen; `create_supervised` stöder redan detta |

### Steg 5: Utvärdering – "the hard part"
Enligt Cerqueira et al. (2020):

- **Aldrig slumpmässig k-fold** – då tränar modellen på framtiden och testas på det förflutna.
- Använd **rolling origin / expanding window backtesting**: träna på allt fram till tidpunkt *t*, förutsäg *t+1…t+H*, flytta *t* framåt och upprepa. Ger många utvärderingspunkter i stället för en enda.
- Spara de sista åren som ett **orört testset**. Hyperparametrar väljs bara på valideringsdata före det.
- All förbehandling (skalning, detrending) ska anpassas **endast på träningsdelen** i varje steg.
- **Mått:** MAE, RMSE och gärna **MASE** (felet relativt seasonal naive – under 1 betyder att vi slår baslinjen).

### Steg 6: Horisontens effekt
- Kör samma modeller för t.ex. H = 4, 12, 26, 52 veckor.
- Plotta **fel som funktion av h**.
- Hypotes: korta horisonter → modeller med lags vinner. Långa horisonter → alla konvergerar mot "genomsnittlig säsongsprofil" eftersom vädret inte går att förutsäga utan väderdata.

## 6. Bredare diskussion (till presentation och rapport)

- Är ML rätt verktyg här, eller räcker en enkel säsongsmodell?
- Vad hade väderdata/temperaturprognoser tillfört? (Fysikaliska modeller: graddagar.)
- Klimatförändringens synlighet i datan.
- Energibolagens nytta: planering av fjärrvärmeproduktion, minskade utsläpp.
- Miljökostnad för att träna tunga modeller vs. nyttan.

## 7. Öppna frågor att besluta i gruppen

- [ ] Vilken **huvudhorisont** kör vi? (Förslag: 12 veckor, plus en kurva 4–52.)
- [ ] Hur stort **testset**? (Förslag: sista 2–5 åren.)
- [ ] Vilka **modeller** är "måste" och vilka är "om tid finns"?
- [ ] **Colab eller lokalt** – vilken miljö kör vi gemensamt?
- [ ] **Arbetsfördelning** – vem tar vad?

## 8. Förslag på arbetsfördelning

| Område | Ansvarig |
|---|---|
| Gemensamt backtest-ramverk (byggs först!) | _ |
| EDA + baslinjer | _ |
| Statistiska modeller (ETS, SARIMA) | _ |
| ML-modeller | _ |
| Prognosstrategier + horisontanalys | _ |
| Presentation | alla |
