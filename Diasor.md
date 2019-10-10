---
title: Git egylet
author: Forrai Benedek, Révész Levente
date: 2019-10-10
...


# Mire jó a verziókezelő?

```
dokumentum.txt
dokumentum1.txt
dokumentum2.txt
dokumentum2.1.txt
dokumentum2.2.txt
dokumentum2.2-kész.txt
dokumentum2.2-kész-végleges.txt
dokumentum2.2-kész-végleges2.txt
dokumentum2.2-kész-végleges2-végleges.txt
```


# A git eredete

Linus Torvalds 2005

A Linux kernel fejlesztéséhez.

\vspace{1cm}

manpage:  
```
NAME
       git - the stupid content tracker
```


# A git jellemzői

- Elosztott
- Free and Open Source Software (FOSS)
- Népszerű


# Telepítés

## Windows
https://git-scm.com/downloads

  - Git Bash terminál
  - nano editor

## Mac
https://git-scm.com/downloads

## Linux
`sudo apt install git`

## Névjegy beállítása

```bash
$ git config --global user.name "Róbert Mogi"
$ git config --global user.email "email@example.com"
```


# Commit

- ,,mentés gomb''
- változásokat tárol

Gráfon:
```
A---B---C---D---E---F
```


# Branch

- alternatív verziók
- egymástól függetlenül módosíthatók

Gráfon:
```
          A---B---C mellekag
         /
A---B---C---D---E---F master
```