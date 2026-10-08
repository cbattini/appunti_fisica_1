# Setup LaTeX (macOS + VS Code)

Guida per compilare questi appunti su macOS con VS Code e l'estensione **LaTeX Workshop**.

## 1. Installare una distribuzione TeX

Due opzioni:

| Distribuzione | Dimensione | Note |
|---|---|---|
| **BasicTeX** | ~100 MB | Minimale: `latexmk` e molti pacchetti vanno installati a mano (vedi sotto) |
| **MacTeX** (no GUI) | ~5 GB | Completa: funziona tutto subito, nessun pacchetto da aggiungere |

```bash
# opzione leggera
brew install --cask basictex

# oppure opzione completa
brew install --cask mactex-no-gui
```

Dopo l'installazione apri un nuovo terminale e verifica:

```bash
ls /Library/TeX/texbin/ | grep -E "latexmk|pdflatex"
```

Con MacTeX devono comparire sia `latexmk` che `pdflatex`: puoi saltare al punto 4.
Con BasicTeX compare solo `pdflatex`: continua dal punto 2.

## 2. Installare latexmk (solo BasicTeX)

LaTeX Workshop usa `latexmk` per compilare, ma BasicTeX non lo include.

```bash
sudo tlmgr update --self
sudo tlmgr install latexmk
```

## 3. Installare i pacchetti mancanti (solo BasicTeX)

Pacchetti caricati da `appunti-base.sty` che non fanno parte del nucleo di LaTeX (quelli già presenti vengono saltati da tlmgr):

```bash
sudo tlmgr install physics siunitx cancel xcolor pgf enumitem booktabs \
  hyperref needspace titlesec fancyhdr microtype lm mathtools babel-italian \
  tcolorbox environ trimspaces tikzfill pdfcol listings listingsutf8 \
  soul tikz-3dplot
```

Per installare in un colpo solo tutto ciò che viene caricato dai sorgenti, esegui dalla root della repository:

```bash
grep -ohE '\\(RequirePackage|usepackage)(\[[^]]*\])?\{[^}]+\}' appunti-base.sty */*.tex \
  | sed -E 's/.*\{([^}]+)\}/\1/' | tr ',' '\n' | tr -d ' ' | grep -v / | sort -u \
  | xargs sudo tlmgr install
```

Alcuni nomi possono dare `not present in repository`: succede quando il nome del file `.sty` non coincide con quello del pacchetto tlmgr. I casi più comuni:

| `\usepackage{...}` | Pacchetto tlmgr |
|---|---|
| `tikz` | `pgf` |
| `tcolorbox` (con opzione `most`) | `tcolorbox` + `environ` + `trimspaces` + `tikzfill` + `pdfcol` + `listings` + `listingsutf8` |
| `tikzfill.image.sty` (errore dalla libreria skins di tcolorbox) | `tikzfill` |

## 4. VS Code

1. Installa l'estensione **LaTeX Workshop** (James Yu).
2. Chiudi VS Code del tutto con **Cmd+Q** e riaprilo: serve perché l'estensione rilegga il `PATH`.
3. Apri `CINEMATICA/cinematica.tex` e salva (o `Cmd+Option+B`) per compilare.

## Risoluzione problemi

### `Error: spawn latexmk ENOENT`

VS Code non trova `latexmk`.

- Se `ls /Library/TeX/texbin/ | grep latexmk` non stampa nulla: manca l'eseguibile, vedi punto 2.
- Se lo stampa: VS Code ha un ambiente vecchio, chiudilo con Cmd+Q e riaprilo.

### `! LaTeX Error: File 'xxx.sty' not found.`

Manca un pacchetto. Prova prima con il nome del file:

```bash
sudo tlmgr install xxx
```

Se tlmgr non lo trova, cerca quale pacchetto contiene quel file e installa quello:

```bash
tlmgr search --global --file xxx.sty
```

### La compilazione continua a fallire dopo aver sistemato l'errore

Pulisci i file ausiliari e ricompila:

```bash
latexmk -C
```

### Aggiornare i pacchetti

```bash
sudo tlmgr update --self --all
```
