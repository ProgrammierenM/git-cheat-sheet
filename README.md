# Git in der Shell // Befehle und Workflow

## git init

Initialisieren eines neuen Git Repository’s

## git status

Prüft den Zustand aller Projektdateien

```
git status // Gibt den Zustand aller Dateien zurück
git status -s // Kompaktere Auflistung
```

## git add

Fügt Dateien zum Staging hinzu, welche für den nächsten Commit vorgesehen werden.

```
git add <Datei> // Setzt eine einzelne Datei auf Staging
git add . // Setzt den aktuellen Zustand aller Dateien auf Staging

git add --patch <Datei> // Einzelne Hunks auswählen
```

## git commit

Erstellt eine neue Version aus den Dateien welche sich im Staging befinden

```
git commit -m "Nachricht" // Erstellt eine neue Version aus den Dateien im Staging
git commit -am "Nachricht" // Überspringt Staging, Neue Dateien werden ignoriert
```

---

## git log

Zeigt die letzten Commits an

```
git log // Zeigt die letzten Commits an
git log -n 3 // Zeigt die letzten 3
git log --graph // Commits mit Graphen
git log --oneline --decorate --graph --all

git reflog // Zeigt alle Referenzen an
```

## git diff

Zeigt alle Unterschiede an zwischen Workspace und Lokales Repository

```
git diff // Unterschiede anzeigen lassen
git diff --staged // Für Dateien im Staging
```

---

## git reset

Änderungen einer/mehreren Dateien und Commits Rückgängig machen

```
git reset // Holt alle Dateien aus Staging und behält alle Änderungen
git reset <Datei> // gleiches, nur auf angegebene Datei
```

## git restore

Änderungen einer Datei Rückgängig machen

```
git restore --staged <Datei> // Holt eine Datei aus Staging und behält alle Änderungen.
git restore <Datei> // Setzt alle Änderungen einer Datei zurück

git restore --staged --patch <Datei> // Einzelne Hunks auswählen
```

## Letzte Commit-Message ändern

```
git commit --amend
// Anschließend öffnet sich dein normaler Editor und
// du kannst eine neue Commit-Message eingeben
```

## Eine Änderung dem letzten Commit hinzufügen

```
// Mach deine Änderung
git add . // oder füge einzelne Dateien hinzu
git commit --amend --no-edit
// Jetzt enthält dein letzter Commit auch die neuen Änderungen!
// WARNUNG: Niemals solltest du "--amend" bei einem
// Commit verwenden, der schon gepusht wurde (es sei denn
// du bist der einzige Entwickler in dem Repo)
```

## Letzten Commit Rückgängig machen

```
git reset --soft HEAD~ // Macht den letzten Commit rückgängig, --> Staging
git reset HEAD~ // letzter Commit Rückgangig, --> Workspace
git reset --mixed HEAD~ // gleiches wie zuvor
git reset --hard HEAD~ // Löscht alle Änderungen des letzten Commits
```

## Älteren Commit Rückgängig machen

```
// Finde den betreffenden Commit
git log
// Verwende die Pfeiltasten um in der History zu scrollen
// und kopiere dir den Hash des betreffenden Commits
git revert [betreffender hash]
// git erstellt einen neuen Commit, der den gewählten
// Commit rückgängig macht. Du musst dafür noch eine
// Commit-Message eingeben oder einfach abspeichern
```

## Änderungen einer einzelnen Datei Rückgängig machen

```
// Finde den Hash eines Commits vor deinen Änderungen
git log
// Verwende die Pfeiltasten um in der History zu scrollen
// und kopiere dir den entsprechenden Hash
git checkout [gewählter hash] -- pfad/zur/datei
// Die alte Version ist jetzt wiederhergestellt
git commit -m "Änderungen an Datei XY Rückgängig gemacht"
```

## Etwas komplett verkackt?

Damit kannst du Dateien zurückholen, die du gelöscht hast, oder Dinge rückgängig machen, die dein Repo zerstört haben, oder einen nicht geglückten Merge oder einfach zu einem Stand zurückkehren, als bestimmte Dinge noch funktioniert haben

```
git reflog
// Du siehst eine Liste mit allem, was du in
// git getan hast, in allen Branches.
// Jeder Eintrag hat einen Index: HEAD@{index}
// Finde den Eintrag VOR demjenigen, der alles
// kaputt gemacht hat
git reset HEAD@{index}
// Alles ist jetzt wieder wie es vorher war
```

---

## git branch

Anzeigen, Erstellen und Löschen von Branches

```
git branch <Name> // Erstellt einen neuen Branch
git branch -l // Listet alle Branches auf
git branch -d <Name> // Branch löschen
```

## git checkout

Erstellen und switchen von Branches

```
git checkout -b <Name> // Erstellen eines neuen Branches
git checkout <Name> // Wechseln in den angegeben Branch
```

## git merge

Führt mehrere Entwicklungsstände zusammen

```
git merge <Branch> // Merged angegeben Branch in den aktuellen hinein

```

## Ausversehen auf den master commited

```
// Erstelle einen neuen Branch mit dem Stand des master
git branch neuer-branch-name
// Entferne den letzten Commit vom master
// und wechsel zum neuen Branch
git reset HEAD~ --hard
git checkout neuer-branch-name
// Dein Commit lebt jetzt in dem neuen Branch weiter :)
```

## Im falschen Branch commited

```
// Mach den letzten Commit rückgängig, aber erhalte die
// Änderungen
git reset HEAD~ --soft
git stash
// Navigiere zum richtigen Branch
git checkout name-des-richtigen-branch
git stash pop
git add . // oder füge einzelne Dateien hinzu
git commit -m "Deine Nachricht hier"
// Jetzt sind die Änderungen auf dem richtigen Branch
```

```
git checkout name-des-richtigen-branch
// Wähle den letzten Commit vom master
git cherry-pick master
// Und lösche den Commit vom master
git checkout master
git reset HEAD~ --hard
```

---

## git remote

Mit Remote-Repositories arbeiten.

```
git remote add origin <URL> // fügt den remote origin hinzu
git remote // zeigt den aktuellen remote origin
git remote get-url origin // zeigt die Remote URL
git remote show origin // Zeigt alle Informationen zu einem Remote
git remote -v // Alle Remotes mit URL's anzeigen
git remote rename origin mario // Remote umbenennen
git remote remove <name> // entfernt den remote origin
```

## git clone

Ein Remote-Repository lokal klonen/herunterladen. Gesamte git history von jeder Datei des Projektes wird heruntergeladen - .git Ordner initialisiert. Die aktuellste Arbeitskopie wird automatisch ausgecheckt. Somit kann auch ein Projekt welches Online beschädigt wird wiederhergestellt werden.

```
git clone <url> // klont das Remote-Repository
git clone <url> <verzeichnis> // Name des Projektverzeichnisses bestimmen
git clone <url> ./ // aktuelles Vereichnis verwenden

git remote -v // Schauen was drin steht nach dem klonen
```

## git push

```
git push // push mit default Werten
git push -u origin // pushed das lokale Repository ins remote. -u ist der upstream
```

## git fetch

Holt den Stand aus dem Online Repository

```
git fetch origin // Stand holen

git merge origin/master // merged den lokalen Stand mit dem neuen Stand
git rebase origin/master // Falls lokal Commits vorhanden sind
```

## git pull

Holt den Online-Stand und merged ihn automatisch mit dem lokalen Stand

```
git pull origin // Holt den Online Stand
git pull origin <Branch> // Holt einen bestimmten Online Branch
```
