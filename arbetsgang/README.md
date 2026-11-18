# Arbetsgång – så har du alltid det senaste

Rutinen för varje gång du sätter dig och jobbar med projektet.
Kör kommandona i terminalen (eller i VS Codes terminal: ``Ctrl + ` ``).

```
1. Gå till projektet  →  2. Hämta senaste main  →  3. Gå till din branch  →  4. Jobba  →  5. Spara & pusha
```

---

## 1. Gå till projektmappen

```bash
cd ~/aiproject/TDDE56-Project
```

(Byt sökvägen till där du själv har klonat repot.)

## 2. Hämta det senaste från GitHub

Stå på `main` och pulla:

```bash
git switch main
```

```bash
git pull
```

Nu har du allt som de andra har slagit ihop.

## 3. Gå till den branch du ska jobba på

**Ny uppgift** → skapa en ny branch (`namn/vad-du-gör`, t.ex. `anna/sarima`):

```bash
git switch -c namn/vad-du-gör
```

**Fortsätter på en befintlig branch** → byt till den och ta in det nya från `main`:

```bash
git switch namn/vad-du-gör
```

```bash
git merge main
```

## 4. Jobba

Öppna projektmappen i VS Code och kontrollera att notebooken använder kerneln **.venv** (uppe till höger).

Har någon lagt till paket i `requirements.txt`? Kör:

```bash
source .venv/bin/activate
```

```bash
pip install -r requirements.txt
```

## 5. Spara och pusha när du är klar

```bash
git add .
```

```bash
git commit -m "Kort beskrivning av vad du gjort"
```

```bash
git push
```

Första gången du pushar en ny branch:

```bash
git push -u origin namn/vad-du-gör
```

## 6. När uppgiften är klar – Pull Request

Gå till repot på GitHub → **Compare & pull request** → beskriv vad du gjort → **Create pull request**.
När den är granskad: **Merge pull request**. Gå sedan tillbaka till steg 2.

---

## Snabbversion

```
cd ~/aiproject/TDDE56-Project
git switch main
git pull
git switch namn/min-branch
git merge main
# ... jobba ...
git add .
git commit -m "Vad jag gjort"
git push
```

## Tips

- **`git status`** – visar vilken branch du står på och vad som är ändrat. Kör den när du är osäker.
- **`git pull` klagar på ändringar som inte är committade** → committa dem först, eller lägg undan dem tillfälligt med `git stash` (hämta tillbaka med `git stash pop`).
- **Push blir "rejected"** → någon annan har pushat till samma branch. Kör `git pull` och sedan `git push` igen.
- Du ser alltid vilken branch du är på **nere till vänster** i VS Code.

Mer om Git: [CONTRIBUTING.md](../CONTRIBUTING.md) · Miljön: [SETUP.md](../SETUP.md)
