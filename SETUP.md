# Utvecklingsmiljö – VS Code

Vi kör allt i **Visual Studio Code (VS Code)**. Där har man notebooks, Python, terminal och Git i
samma fönster, och alla i gruppen får samma upplägg.

Del 1 gör du **en gång**. Del 2 är rutinen för **varje gång** du sätter dig och jobbar.

---

## Del 1 – Engångsinstallation

### 1. Installera programmen

| Program | Var | Kommentar |
|---|---|---|
| **VS Code** | https://code.visualstudio.com/ | Själva editorn |
| **Python 3.11 eller 3.12** | https://www.python.org/downloads/ | Mac: finns ofta redan, testa `python3 --version` i terminalen |
| **Git** | https://git-scm.com/ | Mac: finns ofta redan, testa `git --version` |

> **Windows:** bocka i *"Add Python to PATH"* när du installerar Python.

### 2. Klona repot

Öppna en terminal (Mac: *Terminal*, Windows: *Git Bash* eller *PowerShell*) och gå till mappen där du vill ha projektet:

```bash
git clone https://github.com/JoesephA/TDDE56-Project.git
```

Alternativ utan terminal: i VS Code, tryck `Cmd/Ctrl + Shift + P` → skriv **Git: Clone** → klistra in länken ovan.

### 3. Öppna mappen i VS Code

*File → Open Folder…* → välj mappen `TDDE56-Project`.

Öppna alltid **hela mappen**, inte bara en enskild fil – annars hittar VS Code inte inställningarna och Python-miljön.

### 4. Installera de rekommenderade tilläggen

När du öppnar mappen första gången dyker en ruta upp nere till höger:
*"Do you want to install the recommended extensions?"* → klicka **Install**.

Missade du den: gå till Extensions (fyrkantsikonen i vänstermenyn) och skriv `@recommended`.

| Tillägg | Varför |
|---|---|
| **Python** (Microsoft) | Kör Python, väljer miljö, felsökning |
| **Pylance** | Autokomplettering och felmarkering |
| **Jupyter** | Kör `.ipynb`-notebooks direkt i VS Code |
| **Data Wrangler** | Visa och utforska DataFrames som tabeller |
| **GitLens** | Se vem som ändrat vad, enklare Git-historik |

### 5. Skapa Python-miljön (virtual environment)

En *virtual environment* (`.venv`) är en egen Python-installation bara för det här projektet, så att
alla har samma paketversioner och inget krockar med annat på datorn.

Öppna terminalen i VS Code: *Terminal → New Terminal* (eller ``Ctrl + ` ``). Kör:

**Mac / Linux:**

```bash
python3 -m venv .venv
```

```bash
source .venv/bin/activate
```

```bash
pip install -r requirements.txt
```

**Windows (PowerShell):**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

När miljön är aktiv står det `(.venv)` först på raden i terminalen.

Alternativ utan terminal: `Cmd/Ctrl + Shift + P` → **Python: Create Environment** → **Venv** → välj din Python → bocka i `requirements.txt`.

### 6. Lägg in datan

Hämta `buildings_data.csv` från Lisam och lägg den i en mapp som heter `data/` i projektet:

```
TDDE56-Project/
└── data/
    └── buildings_data.csv
```

Mappen ignoreras av Git, så datan laddas aldrig upp till GitHub.

### 7. Testa att det fungerar

Öppna `heating_forecast.ipynb`. Klicka på **Select Kernel** uppe till höger → **Python Environments** → välj **.venv**.
Kör första cellen med `Shift + Enter`. Får du inga fel är du klar.

> Startnotebooken är skriven för Google Colab. Ta bort raderna `from google.colab import drive` och
> `drive.mount(...)`, och läs datan med `pd.read_csv("data/buildings_data.csv")`.

---

## Del 2 – Rutin varje gång du jobbar

```
 ┌─────────────┐   ┌────────────┐   ┌──────────────┐   ┌───────┐   ┌────────────────┐
 │ 1. Öppna    │ → │ 2. Hämta   │ → │ 3. Rätt      │ → │ 4.    │ → │ 5. Spara &     │
 │ VS Code     │   │ senaste    │   │ branch       │   │ Jobba │   │ pusha          │
 └─────────────┘   └────────────┘   └──────────────┘   └───────┘   └────────────────┘
```

### 1. Öppna projektmappen i VS Code
*File → Open Recent → TDDE56-Project*.

### 2. Hämta det senaste från GitHub

```bash
git switch main
```

```bash
git pull
```

### 3. Gå till din branch och ta in det senaste från main

Ny uppgift? Skapa en ny branch:

```bash
git switch -c ditt-namn/vad-du-gör
```

Fortsätter på en befintlig? Byt till den och ta in det nya från main:

```bash
git switch ditt-namn/vad-du-gör
```

```bash
git merge main
```

Du ser alltid vilken branch du är på **längst ner till vänster** i VS Code. Klicka där för att byta.

### 4. Jobba
- Kontrollera att notebooken använder kerneln **.venv** (uppe till höger).
- Terminalen i VS Code aktiverar `.venv` automatiskt – syns som `(.venv)`.
- Har någon lagt till nya paket i `requirements.txt`? Kör `pip install -r requirements.txt` igen.

### 5. Spara och pusha

Via terminalen:

```bash
git add .
```

```bash
git commit -m "Kort beskrivning av vad du gjort"
```

```bash
git push
```

Eller via VS Code: klicka på **Source Control**-ikonen (grenikonen i vänstermenyn) → skriv ett meddelande → **Commit** → **Sync Changes**.

När din del är klar: skapa en **Pull Request** på GitHub så att den kan slås ihop med `main`. Se [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Bra kortkommandon i VS Code

| Kommando | Mac | Windows |
|---|---|---|
| Kommandopaletten (allt finns här) | `Cmd + Shift + P` | `Ctrl + Shift + P` |
| Öppna/stäng terminal | ``Ctrl + ` `` | ``Ctrl + ` `` |
| Kör cell i notebook | `Shift + Enter` | `Shift + Enter` |
| Sök fil | `Cmd + P` | `Ctrl + P` |
| Source Control (Git) | `Ctrl + Shift + G` | `Ctrl + Shift + G` |

## Vanliga problem

**"ModuleNotFoundError: No module named …"**
Fel kernel eller miljön är inte installerad. Välj **.venv** som kernel och kör `pip install -r requirements.txt`.

**VS Code hittar inte .venv**
`Cmd/Ctrl + Shift + P` → **Python: Select Interpreter** → välj den som slutar på `.venv`.

**Windows: "running scripts is disabled on this system"**
Kör en gång i PowerShell: `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`.

**Notebooken hittar inte datan**
Kolla att filen ligger i `data/buildings_data.csv` och att du öppnat **hela projektmappen** i VS Code.
