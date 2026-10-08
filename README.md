# Appunti di Fisica 1

Appunti del corso di **Fisica 1** (Corso A) dell'Università di Pisa, a.a. 2026/2027, scritti in LaTeX.

- **Docente:** Fabrizio Cei
- **Esercitazioni:** Bischetti

Gli appunti sono presi a lezione e poi integrati e riordinati dopo ogni lezione. Sono in continuo aggiornamento durante il semestre.

> **Nota:** questi sono appunti personali, non materiale ufficiale del corso, e non sono stati revisionati dai docenti. Possono contenere errori o imprecisioni: se ne trovi, apri una issue.

## Contenuti

| Argomento | Sorgente | Stato |
|---|---|---|
| Cinematica | `CINEMATICA/cinematica.tex` | In corso |

Gli argomenti successivi verranno aggiunti man mano che il corso procede.

## Struttura della repository

```
.
├── appunti-base.sty    # stile comune a tutte le materie: layout, box, comandi
├── palette-fisica.tex  # colori e nome della materia
├── template.tex        # scheletro da copiare per un nuovo argomento
├── CINEMATICA/
│   └── cinematica.tex  # appunti di cinematica
├── STILE.md            # guida a stile e palette
├── SETUP-LATEX.md      # guida per installare LaTeX e compilare
├── .gitignore
└── README.md
```

Stile e palette stanno nella root; ogni argomento ha la sua cartella con il proprio `.tex`.

`appunti-base.sty` raccoglie tutto ciò che è comune ai vari file: impostazioni di pagina, pacchetti matematici (`amsmath`, `physics`, `siunitx`), schemi e disegni in TikZ, box per definizioni, teoremi, formule ed esempi. I colori stanno a parte, in `palette-fisica.tex`. Ogni file di appunti li carica dalla cartella superiore:

```latex
\usepackage{../appunti-base}
\input{../palette-fisica}
```

Ambienti, comandi e palette sono descritti in [STILE.md](STILE.md).

### Aggiungere un argomento

```bash
mkdir DINAMICA
cp template.tex DINAMICA/dinamica.tex
```

Poi si sostituiscono i segnaposto in `dinamica.tex` e si aggiunge una riga alla tabella dei contenuti. `template.tex` si compila solo da dentro una cartella di argomento, perché carica stile e palette con `../`.

## Compilare gli appunti

Serve una distribuzione TeX con `latexmk`. La procedura completa per macOS e VS Code, compresi i pacchetti da installare con BasicTeX e la soluzione degli errori più comuni, è in [SETUP-LATEX.md](SETUP-LATEX.md).

Da terminale, dalla cartella dell'argomento (i percorsi `../` sono relativi a quella):

```bash
cd CINEMATICA
latexmk -pdf cinematica.tex
```

Per eliminare i file ausiliari:

```bash
latexmk -c
```

Con VS Code e l'estensione LaTeX Workshop basta aprire il file `.tex` e salvare.

## Segnalazioni e contributi

Errori, refusi e passaggi poco chiari si possono segnalare aprendo una [issue](../../issues). Le pull request con correzioni sono benvenute.
