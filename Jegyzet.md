# Git kezdőknek

## Mire jó a verziókezelő?

Számítógépes munka közben gyakran előfordul, hogy egy fájlon dolgozik az ember, de bizonyos fájlverziókat szeretne külön lementeni. Így jönnek létre a **fájl-v1**, **fájl-v2**, **fájl-v99** jellegű állományok a számítógépünkön, amelyekből sokszor magunk se tudjuk, hogy melyiknek mi a tartalma. Fejlesztés közben pedig legtöbbször elvárás, hogy egy működőképes verzió mindig elérhető legyen, a fejlesztést ne közvetlenül ezen végezzük. Túl sok verzió esetén viszont a projekt szétszóródik, kezelhetetlenné válik.

A verziókezelők erre a problémára nyújtanak megoldást. Ezek a programok struktúráltan eltárolják a projekt különböző verzióit. Lehetővé teszik, hogy ,,pillanatképeket'' készítsünk a projektünkről, és ezekhez bármikor visszatérhessünk. A projektnek nem mentik le többszéz példányát, hanem mindig csak a változások kerülnek tárolásra, így a tárhelyet nem pazaroljuk. Lehetővé teszik több verzió párhuzamos használatát, és eszközöket nyújtanak a kollaborációra, így egyszerre többen is dolgozhatunk ugyanazon projekten. A különböző emberektől származó módosításokat a verziókezelő magától egybegyúrja egy közös verzióvá.


## A git eredete

A git verziókezelőt Linus Torvalds készítette 2005-ben, a Linux kernel fejlesztéséhez. A manual oldalán így mutatkozik be:

```
NAME
       git - the stupid content tracker
```

A név valószínűleg nem jelentett semmit, talán azért lett git, mert a _get_ szóra kicsit hasonlít, és még nem volt ilyen UNIX parancs.


## A git jellemzői

**Elosztott**:
A felépítése nem kliens-szerver jellegű, hanem minden git mappa egy teljes értékű repository, amelyből lehet leölteni, feltölteni.

**Free and Open Source Software (FOSS)**:
A git a Linux világból ismert *GNU General Public License V2* alatt terjesztett szoftver, azaz szabadon felhasználható, sosem lesz fizetős.

**Népszerű**
A git használata egyszerű, logikus, könnyen elsajátítható. Ez a legnépszerűbb verziókezelő.

---


## Telepítés

#### Windows

1. A git telepítő letöltése a weboldalról: https://git-scm.com/downloads
2. Telepítés
3. A telepítőben lehet terminált választani: cmd, PowerShell vagy Git Bash. Válasszuk a **Git Bash** opciót.
4. A telepítőben lehet szövegszerkesztőt választani. Válasszuk a **nano** vagy a **Notepad++** opciót (utóbbit csak ha telepítve van).

A telepítés után a **Git Bash** terminálban lehet használni a git eszközöket.

#### Linux

Írd be a terminálba:

```bash
$ sudo apt install git
```

#### Mac

Az Xcode telepítésével a git is felkerül a gépre. Próbáld ki.

```bash
git --version
```

Ha nem találja a parancsot, telepítsd az Xcode-ot, vagy csak a gitet a weboldalról: https://git-scm.com/downloads

### Névjegy beállítása

A git a tevékenységedet egy névvel és egy email címmel írja alá. Általában a tényleges nevünket szoktuk megadni.

```bash
$ git config --global user.name "Mona Lisa"
$ git config --global user.email "email@example.com"
```

## Alapok

A git egyik alapfogalma a **commit**. A commit tulajdonképpen egy ,,mentés'' gomb. A projektünk aktuális állapotáról készít egy pillanatfelvételt és azt eltárolja. Egy adott commithoz bármikor visszatárhetünk. A commit az előző commit óta végzett változásokat menti el.

A commitok tehát egy gráfon ábrázolva így néznek ki:

```
A---B---C---D---E---F
```

Ezen fő ág mellett létrehozhatunk egy leágazást, egy alternatív verziót. A különböző ágak neve **branch**, ez a git másik alapfogalma. Az ág az elágazásbeli állapotot megörökli a törzsétől, azután pedig a törzstől függetlenül módosítható.

```
          A---B---C mellekag
         /
A---B---C---D---E---F master
```


## Használat


### Repository készítése

Egy új repository indításához ennyit kell beírnunk a parancssorba:

```bash
$ git init
Initialized empty Git repository in /home/levente/Dev/sandbox/git-proba/.git/
```

Ez az adott mappában létrehoz egy .git nevű rejtett mappát, amelyben a saját fájljait tárolja. Az inicializálás után ebben a mappában használhatjuk az összes többi git parancsot.

Az induláshoz nincs szükségünk semmilyen fájlra, de egy README fájlt szokás készíteni rögtön az elején. Neve lehet README vagy README.md, tartalmához használhatjuk a [Markdown] szintaxist. Írjuk bele a repository nevét, rövid leírását.

[Markdown]: https://guides.github.com/features/mastering-markdown/


### Status

Készítsünk pár fájlt a mappánkban.

```bash
$ echo "print('Hello')" >> Hello.py
$ echo "print('Goodbye')" >> Goodbye.py
```

Gyorsan megnézhetjük a mappánk tartalmát az `ls` paranccsal.

```bash
$ ls
Goodbye.py  Hello.py  README.md
```

Nézzük meg a fájlok állapotát a `git status` parancs segítségével!

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	Hello.py
	Goodbye.py

nothing added to commit but untracked files present (use "git add" to track)
```

Ez elég sok infót közöl velünk:

- Melyik ágban vagyunk éppen. A fő ág neve **master**.
- Melyik fájlok változtak meg az előző commit óta.
- Melyik megváltozott fájlok vannak hozzáadva a következő commithoz. Jelenleg egy sincs.

### Staging

A fájlokat szeretnénk hozzáadni a következő commithoz. Erre az `git add` parancs szolgál, így kell használni:

```bash
$ git add Hello.py Goodbye.py
```

Nézzük meg újra a státuszt!

```bash
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   Goodbye.py
	new file:   Hello.py
```

Látjuk, hogy a változásaink benne lesznek a következő commitban. Azt a környezetet, amibe ilyenkor bekerülnek, **staging**nek hívjuk. A hozzáadott fájlokat az angol **staged** jelzővel illetjük.

Minden fájl hozzáadásához nem kell az összeset felsorolni, elég ennyit írni.

```bash
$ git add .
```


### Commit

A staging környezetben lévő fájlokat lementhetjük a `git commit` paranccsal.

```bash
$ git commit
```

Minden commit tartalmaz egy üzenetet (**commit message**), ami tömören összefoglalja a változások célját. A következő igékkel szokás kezdeni: Add, Update, Remove, Refactor, Optimize, Fix. Ezután pedig a módosított dolgokat foglald össze.  
Példa: "Add Hello script".

A `git commit` parancs megnyit egy szövegszerkesztőt (általában a **nano** editort). A megnyíló fájl kommentként tartalmazza a megváltoztatott fájlok nevét. A felső sorba írjuk be az üzenetet: "Add Hello and Goodbye scripts". Mentsük (**Ctrl+S**), majd zárjuk be a szerkesztőt (**Ctrl+X**).

A commit elkészült, egy hasonló összegzést láthatunk a terminálablakban:

```
[master (root-commit) 99dd3d3] Add Hello and Goodbye scripts
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 Goodbye.py
 create mode 100644 Hello.py
```

Az üzenetet a parancssorban is megadhatjuk:

```bash
$ git commit -m "Update Hello script"
```

### Log

Az eddigi commitjainkat a `git log` parancs mutatja meg:

```
$ git log
commit af14db18e222d63b5aa2df167c788c4932d91e82 (HEAD -> master)
Author: leventerevesz <levete.revesz@gmail.com>
Date:   Fri Oct 4 09:51:52 2019 +0200

    Add Hello and Goodbye scripts
```

### Diff

Módosítsunk az egyik fájlon! A Hello.py tartalmát írjuk át:

```python
print("Hello")
print("World")
```
Mentsük el!

A `git status` jelzi nekünk, hogy a Hello.py fájlban történt módosítás.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   Hello.py

no changes added to commit (use "git add" and/or "git commit -a")
```

A változásokat a `git diff` paranccsal tudjuk megnézni.

```
$ git diff
diff --git a/Hello.py b/Hello.py
index 2f9a147..3d3d93b 100644
--- a/Hello.py
+++ b/Hello.py
@@ -1 +1,2 @@
 print("Hello")
+print("World")
```

A `+` jellel megjelölt sorok újak, a `-` jellel megjelölt sorok törölve lettek. A változás előtt és után lévő sort is kiírja, hogy lássuk a kontextust. Az összehasonlítás alapja a staging környezet, vagyis a már committed vagy added állapotú fájlok. A diff parancs különböző ágakat is össze tud hasonlítani, valamint csak egyes fájlok változásait is megtekinthetjük.

```bash
$ git diff Goodbye.py
```

_Nincs kimenet, mert nincs változás a fájlban._

A legutóbbi commithoz képesti változásokat így nézhetjük meg:

```bash
$ git diff HEAD
```

### Reset

Resetet akkor csinálunk, ha valamit elrontottunk. 

#### (Mixed) reset
A `git add .` ellentéte. A staging környezetet kiüríti, de a lokális módosításaink megmaradnak. 

```bash
$ git reset
```

#### Hard reset
A lokális módosításainkra nincs szükségünk, vissza akarunk térni a legutóbbi commithoz.

```bash
$ git reset --hard
```

### Branch készítés, váltás

Új branchet a `git branch` paranccsal hozhatunk létre. Ennek alapja a legutóbbi commit lesz, és a lokális módosításaink (amiket még nem committeltünk) is átkerülnek bele. 

Ágakat használunk a fejlesztési folyamatok elkülönítésére. A program minden új funkcióját egy külön ágban kezdjük el, így a master ágban mindig egy működő verzió lehet, amiből kollégáink saját mellékágakat indíthatnak a mi félkész kódunktól függetlenül. De egyszemélyes fejlesztés esetén is érdemes külön ágakban fejleszteni a különböző funkciókat.

Hozzunk létre egy mellékágat!

```bash
$ git branch mellekag
```

A `git branch` listázza a létező ágakat, csillaggal jelöli azt amin vagyunk.

```bash
$ git branch
* master
  mellekag
```

Gráfon így néz ki amit műveltünk:

```
              A mellekag
             /
A---B---C---D master
```

Az ágak közötti váltásra a `git checkout` szolgál. Lépjünk át a másik ágra!

```bash
$ git checkout mellekag
```

Megnézhetjük, tényleg átlépett.
```bash
$ git branch
  master
* mellekag
```

Ágak közötti váltás előtt érdemes committálni minden lokális változást, vagy hard resetelni őket, ha nincs rájuk szükség.

### Stash

Képzeljük el, hogy megnyitjuk a forráskódunkat, nagyban írjuk a módosításokat, majd eszünkbe jut, hogy rossz branchen vagyunk. Ennek megoldására szolgál a stash: a lokális változtatásokat lehet félrerakni, és egy másik branchen betölteni.

Módosítások félrerakása

```bash
$ git stash
```

Módosítások félrerakása, és visszatérés a legutóbbi commithoz.

```bash
$ gut stash push
```

Módosítások visszatöltése

```bash
$ git stash pop
```

Tehát ha a mellekag-ra szánt módosítsokat véletlenül a master ágon írtuk, ezt kell tenni:

```bash
git stash push
git checkout mellekag
git stash pop
```

### Merge

A `git merge` egybeolvaszt két ágat. Egy másik ágban készített összes változás átmásolódik az aktuálisan használt ágba. Gráfon így néz ki a művelet:

```
          A---B---C mellekag
         /         \
A---B---C---D---E---M master
```

Az M-mel jelölt commit az ún. merge commit, ez tartalmazza a mellékág változtatásainak összességét.

A gyakorlatban:

```bash
$ git checkout master
$ git merge mellekag
```

Előfordul, hogy bizonyos kódsorokon mindkét ág változtatott, ilyenkor merge conflict lép fel. Ezt kézzel kell feloldanunk, ki kell választani a megtartandó verziót.

A merge után a mellékág nem törlödik, tovább használható.


## Távoli repository használata

GitHub és GitLab

### Origin beállítása

```bash
$ git remote add origin https://github.com/user/repo.git
$ git push -u origin master
```

### Clone

### Pull

### Push