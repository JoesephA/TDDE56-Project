# Så jobbar vi med Git och GitHub

En enkel guide för gruppen. Du behöver inte kunna allt – de viktigaste kommandona är
`pull`, `add`, `commit` och `push`.

---

## Ordlista

| Ord | Betyder |
|---|---|
| **Repo** (repository) | Projektmappen, med hela dess historik |
| **Clone** | Ladda ner repot till din dator första gången |
| **Commit** | En sparad "ögonblicksbild" av ändringar, med ett meddelande som beskriver vad du gjort |
| **Push** | Skicka dina commits från din dator upp till GitHub |
| **Pull** | Hämta andras ändringar från GitHub ner till din dator |
| **Branch** | En egen parallell version av koden där du kan jobba utan att störa andra |
| **Pull Request (PR)** | En förfrågan på GitHub om att slå ihop din branch med `main` – andra kan granska först |
| **Merge** | Slå ihop två brancher |
| **Konflikt** | När två personer ändrat samma rader – Git vet inte vilken version som gäller och du får välja |

---

## 1. Första gången (görs en gång)

**Installera Git** – på Mac finns det oftast redan. Testa:

```bash
git --version
```

**Berätta för Git vem du är:**

```bash
git config --global user.name "Ditt Namn"
```

```bash
git config --global user.email "din@mail.se"
```

**Ladda ner repot:**

```bash
git clone https://github.com/JoesephA/TDDE56-Project.git
```

```bash
cd TDDE56-Project
```

> Du måste vara tillagd som *collaborator* på GitHub för att kunna pusha. Säg till Joseph om du inte är det.
> Vid första push kan GitHub be dig logga in. Enklast är att installera [GitHub CLI](https://cli.github.com/) och köra `gh auth login`, eller använda [GitHub Desktop](https://desktop.github.com/) om du föredrar ett grafiskt program.

---

## 2. Det dagliga arbetsflödet

### Steg 1 – Hämta det senaste innan du börjar

```bash
git pull
```

Gör **alltid** detta först, så att du jobbar på den senaste versionen.

### Steg 2 – Jobba som vanligt
Redigera filer, kör notebooks, osv.

### Steg 3 – Se vad du har ändrat

```bash
git status
```

Visar vilka filer som är ändrade (röda = inte tillagda än, gröna = redo att committas).

### Steg 4 – Välj vilka filer som ska med

```bash
git add filnamn.ipynb
```

eller allt på en gång (kolla `git status` först så du inte får med skräp):

```bash
git add .
```

### Steg 5 – Spara en commit med ett beskrivande meddelande

```bash
git commit -m "Lägg till seasonal naive-baslinje"
```

Skriv vad ändringen *gör*, kort och tydligt. Dåligt: `"fix"`, `"asdf"`. Bra: `"Lägg till MASE-mått i utvärderingen"`.

### Steg 6 – Skicka upp till GitHub

```bash
git push
```

**Sammanfattning:** `pull` → jobba → `add` → `commit` → `push`.

---

## 3. Jobba på en egen branch (rekommenderas)

För att inte skriva över varandras arbete jobbar var och en på en egen branch och slår ihop via Pull Request.

**Skapa och byt till en ny branch:**

```bash
git switch -c joseph/eda
```

Namnförslag: `ditt-namn/vad-du-gör`, t.ex. `anna/sarima`.

**Pusha branchen första gången:**

```bash
git push -u origin joseph/eda
```

Därefter räcker `git push`.

**Skapa en Pull Request:** gå till repot på GitHub – en gul ruta dyker upp med knappen *Compare & pull request*. Beskriv kort vad du gjort, be någon titta, och klicka sedan *Merge*.

**Gå tillbaka till main och hämta det senaste:**

```bash
git switch main
```

```bash
git pull
```

**Uppdatera din branch med det senaste från main:**

```bash
git merge main
```

---

## 4. Vanliga problem

**"rejected … fetch first" när du pushar**
Någon annan har pushat innan dig. Hämta deras ändringar först:

```bash
git pull
```

och pusha sedan igen.

**Merge-konflikt**
Git markerar konflikten i filen så här:

```
<<<<<<< HEAD
din version
=======
deras version
>>>>>>> main
```

Välj vad som ska vara kvar, ta bort markeringarna, och kör sedan `git add filen` och `git commit`.

**Konflikter i notebooks (.ipynb)** är jobbiga eftersom de är stora JSON-filer. Tips:
- Jobba inte två personer i **samma notebook** samtidigt – gör egna notebooks (t.ex. `notebooks/anna_sarima.ipynb`).
- Rensa outputs innan commit om de inte behövs (*Edit → Clear All Outputs*).
- Lägg återanvändbar kod i `.py`-filer som flera notebooks kan importera.

**Ångra ändringar i en fil du inte har committat**

```bash
git restore filnamn
```

(Varning: ändringarna försvinner.)

**Se historiken**

```bash
git log --oneline
```

---

## 5. Gruppregler

1. Kör `git pull` innan du börjar jobba.
2. Committa ofta, med tydliga meddelanden.
3. Pusha **inte datafilen** (`*.csv` är ignorerat i `.gitignore`). Alla hämtar den från Lisam.
4. Jobba helst på egen branch och slå ihop via Pull Request.
5. En person per notebook åt gången.
6. Pusha inte trasig kod till `main` – testa att notebooken kör först.
