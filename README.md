# TDDE56 – Prognos av värmeeffektbehov för byggnader

Grupprojekt i kursen **TDDE56 Foundations of AI and ML** vid Linköpings universitet (HT2 2026).

## Vad projektet handlar om

Ett varmare klimat och skärpta krav på byggnaders energiprestanda påverkar hur mycket värme
bebyggelsen behöver. För energibolag och för långsiktig energipolitik är det därför intressant
att kunna förutsäga framtida värmebehov.

**Mål:** bygga en modell som så noggrant som möjligt prognostiserar det **genomsnittliga veckovisa
värmeeffektbehovet (kW)** för ett bestånd av byggnader. Prognoshorisonten väljer vi själva, men
den måste vara **minst 4 veckor**.

**Huvudfråga:** Hur långt fram kan vi förutsäga ett byggnadsbestånds värmebehov, och hur bra?

## Data

- Veckovis medelvärde av värmeeffekt (kW) för **70 byggnader i Linköping** (fjärrvärme, mätt med värmemätare: flöde × ΔT).
- **En enda tidsserie**, ca **2 140 veckor**, inga förklarande variabler.
- Datafilen (`buildings_data.csv`) finns i Lisam och checkas inte in i repot.

## Uppgifter (lätt → svår)

1. **Bekanta oss med notebooken och datan** – köra `heating_forecast.ipynb`, förstå koden och graferna.
2. **Jämföra olika ML-modeller** och deras prediktionsförmåga, med korrekt utvärdering av tidsseriemodeller (Cerqueira et al., 2020).
3. **Jämföra prognosstrategier** för flerstegsprognoser – t.ex. rekursiv, direkt, MIMO (Taieb et al., 2012).
4. **Studera effekten av prognoshorisonten** – hur förändras noggrannheten när horisonten ökar? Går det att prognostisera långt fram?

Det svåra i projektet: att **utvärdera tidsserier ärligt** (ingen informationsläcka från framtiden).

## Läs vidare

- 📘 **[docs/METODER.md](docs/METODER.md)** – hur vi tänker angripa problemet: begrepp, modeller, prognosstrategier, utvärdering och öppna frågor.
- 🛠️ **[CONTRIBUTING.md](CONTRIBUTING.md)** – hur man använder Git/GitHub i gruppen (clone, pull, commit, push, branches, konflikter).

## Repo-struktur

```
├── heating_forecast.ipynb   # Startnotebook från kursen (laddar data, plottar, skapar supervised-format)
├── requirements.txt         # Python-beroenden för lokal körning
├── CONTRIBUTING.md          # Git-guide för gruppen
└── docs/
    ├── METODER.md                         # Vårt metodupplägg (diskussionsunderlag)
    ├── heating-demand-forecasting.pdf     # Projektbeskrivning
    ├── upg2_group_project_guidelines.pdf  # Riktlinjer för grupprojektet
    └── upg2_project_intro.pdf             # Introduktionsbilder för alla projekt
```

## Kom igång

Vi arbetar via **GitHub**. Alla klonar repot, kör lokalt och jobbar på en **egen branch** som slås ihop med `main` via Pull Request. Se [CONTRIBUTING.md](CONTRIBUTING.md) för steg-för-steg.

**1. Klona repot och skapa din branch:**

```bash
git clone https://github.com/JoesephA/TDDE56-Project.git
cd TDDE56-Project
git switch -c ditt-namn/vad-du-gör
```

**2. Sätt upp Python-miljön:**

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

**3. Lägg datan på plats:** hämta `buildings_data.csv` från Lisam och lägg den i en mapp `data/` i repot (den ignoreras av Git och pushas inte).

**4. Starta Jupyter:**

```bash
jupyter lab
```

Startnotebooken innehåller Colab-specifika rader (`from google.colab import drive`, `drive.mount(...)`). Ta bort dem och läs datan med `pd.read_csv("data/buildings_data.csv")`.

## Viktiga datum

| Vad | När |
|---|---|
| Gruppkontrakt & projektplan (gruppinlämning, Lisam) | fre 13 nov 2026, 23:30 |
| Presentation av resultat (10 min + 5 min frågor) | tors 17 dec 2026, 13:15–17:00 |
| Individuell rapport (~1 500 ord, engelska, Lisam) | lör 16 jan 2027, 23:30 |

## Referenser

- V. Cerqueira, L. Torgo, I. Mozetič. *Evaluating time series forecasting models: An empirical study on performance estimation methods.* Machine Learning, 109:1997–2028, 2020.
- S. Ben Taieb, G. Bontempi, A. F. Atiya, A. Sorjamaa. *A review and comparison of strategies for multi-step ahead time series forecasting based on the NN5 forecasting competition.* Expert Systems with Applications, 39(8):7067–7083, 2012.
- R. J. Hyndman, G. Athanasopoulos. *Forecasting: Principles and Practice* (3rd ed.). https://otexts.com/fpp3/
- [skforecast](https://skforecast.org/) – introduktion till tidsserieprognoser i Python.
- F. Lazzeri. *Machine Learning for Time Series Forecasting with Python* (finns via LiU-biblioteket).
