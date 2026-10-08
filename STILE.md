# Guida a stile e palette

Come usare `appunti-base.sty` (lo stile comune a tutte le materie) e `palette-fisica.tex` (i colori della materia).

L'idea di fondo: **lo stile è uno solo, la palette cambia per materia.** Ambienti, impaginazione e comandi sono identici ovunque; per una nuova materia si scrive soltanto un nuovo file `palette-<materia>.tex`.

## Indice

- [Scheletro di un documento](#scheletro-di-un-documento)
- [Palette](#palette)
- [Box e ambienti](#box-e-ambienti)
- [Comandi in linea](#comandi-in-linea)
- [Schemi e disegni](#schemi-e-disegni)
- [Riepilogo di fine blocco](#riepilogo-di-fine-blocco)
- [Matematica e unità di misura](#matematica-e-unità-di-misura)
- [Impaginazione automatica](#impaginazione-automatica)
- [Cose a cui fare attenzione](#cose-a-cui-fare-attenzione)

## Scheletro di un documento

```latex
\documentclass[11pt]{report}

\usepackage{../appunti-base}
\input{../palette-fisica}     % dopo appunti-base: \definecolor richiede xcolor

\universita{Università di Pisa}
\corso{Corso di Laurea in ...}
\autore{Nome Cognome}
\professore{Prof. ...}
% \aggiornato{8 ottobre 2026} % facoltativo: se omesso usa la data di oggi

\begin{document}

\copertina{Cinematica}        % l'argomento è il titolo grande in copertina
\tableofcontents

\chapter{Moto in una dimensione}
\lezione{7 ottobre 2026}

\section{Posizione e velocità}
...

\end{document}
```

Stile e palette stanno nella root della repository, mentre ogni `.tex` sta nella cartella del suo argomento: per questo si caricano con `../`. Nel log compare l'avviso `You have requested package '../appunti-base', but the package provides 'appunti-base'`, che è innocuo.

Per partire da zero c'è `template.tex` nella root: crea la cartella dell'argomento, copiaci il file, rinominalo e sostituisci i segnaposto. Contiene già un esempio di ogni ambiente.

Si compila con **pdfLaTeX**, dalla cartella dell'argomento. La copertina mostra università, corso, «Appunti di *materia*», argomento, autore, professore e data di aggiornamento.

## Palette

`palette-fisica.tex` definisce il nome della materia e sette colori. Lo stile usa solo questi nomi, mai colori fissi (tranne il rosso delle segnalazioni).

| Nome | Colore | HEX | Dove viene usato |
|---|---|---|---|
| `materia` | blu | `#3F5FA8` | Titoli di sezione, filetti, definizioni, formule, frecce |
| `materiascuro` | blu-ardesia | `#2B3A55` | Titoli di capitolo e sottosezione, `\chiave`, osservazioni |
| `materiachiaro` | azzurro | `#8FA6D8` | Bordo del box «Significato fisico» |
| `accento` | rosso mattone | `#C8452F` | Teoremi, leggi, principi; data della lezione |
| `esempio` | verde | `#2E8B6E` | Box degli esempi |
| `nota` | ambra | `#E0A100` | Box «NB» |
| `evid` | giallo | `#FBE7A1` | Evidenziatore (`\evid`, `nodoevid`) |

I colori si possono usare direttamente nel testo e in TikZ: `\textcolor{materia}{...}`, `\draw[accento] ...`.

### Creare la palette di un'altra materia

1. Copia `palette-fisica.tex` in `palette-<materia>.tex` (per esempio `palette-analisi.tex`), sempre nella root.
2. Cambia `\MateriaNome` e i sette codici HEX.
3. Nel documento sostituisci `\input{../palette-fisica}` con `\input{../palette-<materia>}`.

```latex
\newcommand{\MateriaNome}{Analisi 1}
\definecolor{materia}{HTML}{......}
\definecolor{materiascuro}{HTML}{......}
\definecolor{materiachiaro}{HTML}{......}
\definecolor{accento}{HTML}{......}
\definecolor{esempio}{HTML}{......}
\definecolor{nota}{HTML}{......}
\definecolor{evid}{HTML}{......}
```

Tutti e sette i nomi, più `\MateriaNome`, sono obbligatori: se ne manca uno la compilazione si ferma con «Undefined color». Per la leggibilità conviene tenere `materiascuro` abbastanza scuro da reggere un titolo e `evid` abbastanza chiaro da lasciar leggere il testo nero sopra.

## Box e ambienti

I box hanno fondo bianco, un filetto colorato a sinistra e l'etichetta in grassetto all'inizio del testo, senza barra del titolo.

| Ambiente | Etichetta | Colore | A cosa serve |
|---|---|---|---|
| `definizione` | **Definizione 1.1 (Nome).** | `materia` | Definizioni, numerate per capitolo |
| `teorema` | **Il titolo che scrivi.** | `accento` | Teoremi, leggi, principi (testo in corsivo) |
| `formula` | nessuna | `materia` | Formula chiave riquadrata |
| `significato` | **Significato fisico⊕.** | `materiachiaro` | Spiegazione di una formula, subito dopo il box |
| `esempio` | **Esempio (titolo).** | `esempio` | Esempi ed esercizi svolti |
| `nota` | **NB.** | `nota` | Gli «NB!» degli appunti |
| `delucidazione` | **Delucidazione.** | rosso scuro | Note a margine e scritte rosse |
| `dimostrazione` | *Dimostrazione.* | `materiascuro` | Dimostrazioni e passaggi, senza box, chiuse da □ |
| `osservazione` | **Osservazione.** | `materiascuro` | Osservazioni, senza box |

### Uso

```latex
\begin{definizione}{Velocità media}
  Rapporto tra lo spostamento e l'intervallo di tempo in cui avviene.
\end{definizione}

\begin{teorema}{Legge oraria del moto uniforme}
  In un moto rettilineo uniforme la posizione varia linearmente nel tempo.
\end{teorema}

\begin{formula}
  x(t) = x_0 + v\,t
\end{formula}

\begin{significato}
  La posizione cresce della stessa quantità in tempi uguali.
\end{significato}

\begin{esempio}{Treno in partenza}
  Un treno parte da fermo con accelerazione costante...
\end{esempio}

\begin{nota}
  Vale solo se l'accelerazione è costante.
\end{nota}

\begin{delucidazione}
  Qui il prof intendeva il modulo della velocità.
\end{delucidazione}

\begin{dimostrazione}
  Integrando $a = \dv{v}{t}$ tra $0$ e $t$ si ottiene...
\end{dimostrazione}

\begin{osservazione}
  Il risultato non dipende dalla scelta dell'origine.
\end{osservazione}
```

### Regole degli argomenti

- **`definizione`, `teorema`, `esempio` vogliono sempre le graffe del titolo.** Se il titolo non serve lasciale vuote: `\begin{definizione}{}` stampa solo «Definizione 1.1.», `\begin{esempio}{}` solo «Esempio.».
- **`formula` è già in modo matematico** (è un `equation*`): niente `$...$` né `\[...\]` all'interno. Per più righe usa `aligned`.
- **I box non si spezzano tra due pagine.** Se uno è troppo lungo per stare in una pagina, aggiungi l'opzione `lungo`:
  ```latex
  \begin{esempio}[lungo]{Esercizio completo}
    ...
  \end{esempio}
  ```
- Tra parentesi quadre si può passare qualsiasi opzione di `tcolorbox`, ad esempio `\begin{nota}[colframe=accento]`.
- Le definizioni si possono citare: `\label{def:vmedia}` dentro il box, poi `Definizione~\ref{def:vmedia}`.

## Comandi in linea

| Comando | Risultato | Quando usarlo |
|---|---|---|
| `\evid{testo}` | Sfondo giallo | Parti evidenziate negli appunti. Funziona anche in matematica: `$\evid{v = 0}$` |
| `\chiave{testo}` | Grassetto blu-ardesia | Concetti chiave, sottolineati negli appunti |
| `\implica` | Freccia → colorata | La freccia logica «quindi» degli appunti |
| `\lezione{7 ottobre 2026}` | «Lezione del ...» a destra | All'inizio di ogni lezione |
| `\illeggibile` | **[illeggibile]** in rosso | Parte che non si riesce a leggere |
| `\illeggibile[forse un indice]` | **[illeggibile: forse un indice]** | Come sopra, con un'ipotesi |
| `\incerto{testo}` | Testo rosso con `?` in apice | Trascrizione di cui non si è sicuri |
| `\aggiunto` | ⊕ in apice | Marca ciò che **non** viene dagli appunti |

`\illeggibile` e `\incerto` restano sempre visibili nel PDF e si trovano cercando `illeggibile` o `incerto` nei sorgenti: prima di considerare finito un capitolo conviene controllare che non ne siano rimasti.

**Convenzione ⊕:** tutto ciò che è stato aggiunto rispetto agli appunti originali porta il simbolo ⊕. `significato` e `\riepilogo` lo inseriscono da soli; per un'aggiunta isolata nel testo si scrive `\aggiunto` a mano.

## Schemi e disegni

### Schemi a frecce

L'ambiente `schema` apre già un `tikzpicture` centrato: dentro si scrivono direttamente nodi e frecce.

```latex
\begin{schema}
  \node[nodo]                  (x) {posizione $x(t)$};
  \node[nodoevid, right=of x]  (v) {velocità $v(t)$};
  \node[nodo, right=of v]      (a) {accelerazione $a(t)$};
  \draw[freccia] (x) -- node[above] {$\dv{t}$} (v);
  \draw[freccia] (v) -- node[above] {$\dv{t}$} (a);
\end{schema}
```

| Stile | Effetto |
|---|---|
| `nodo` | Nodo semplice con angoli arrotondati |
| `nodoevid` | Nodo con sfondo evidenziatore |
| `freccia` | Freccia spessa nel colore `materia` |

### Disegni

Per gli schizzi ridisegnati in TikZ. A differenza di `schema`, il `tikzpicture` va scritto esplicitamente, con lo stile `disegno`:

```latex
\begin{disegno}
  \begin{tikzpicture}[disegno]
    \draw[asse] (0,0) -- (5,0) node[right] {$t$};
    \draw[asse] (0,0) -- (0,3) node[above] {$x$};
    \draw[curva] (0,0.5) parabola (4,2.8);
    \draw[tratt] (2,0) -- (2,1.1);
    \node[punto, label=above left:$P$] at (2,1.1) {};
    \draw[vett, inkrosso] (2,1.1) -- ++(1,0.6) node[right] {$\vb{v}$};
  \end{tikzpicture}
\end{disegno}
```

| Stile | Effetto |
|---|---|
| `asse` | Asse con freccia, grigio scuro |
| `curva` | Linea spessa nel colore `materia` |
| `vett` | Vettore: freccia molto spessa |
| `tratt` | Linea tratteggiata grigia (proiezioni, costruzioni) |
| `punto` | Punto pieno |

Colori «penna» per i disegni, collegati alla palette: `inkblu` (= `materia`), `inkrosso` (= `accento`), `inkverde` (= `esempio`), `inkgiallo` (= `nota`).

Un disegno più largo della pagina viene ridotto automaticamente alla larghezza del testo. Sono già caricati `tikz-3dplot` e le librerie `angles`, `quotes`, `3d`, `intersections`, `patterns`, `decorations.markings`, `backgrounds`.

## Riepilogo di fine blocco

Dopo un gruppo di argomenti si può inserire una pagina riassuntiva. `\riepilogo` va sempre a pagina nuova, aggiunge la voce all'indice e porta il simbolo ⊕.

```latex
\riepilogo{Moti rettilinei}

\begin{mappa}
  \node[nodoevid]               (m) {Moto rettilineo};
  \node[nodo, below left=of m]  (u) {uniforme\\$a = 0$};
  \node[nodo, below right=of m] (ua) {unif. accelerato\\$a$ costante};
  \draw[freccia] (m) -- (u);
  \draw[freccia] (m) -- (ua);
\end{mappa}

\begin{formulario}
  x(t) = x_0 + v\,t                    & Moto uniforme: spazi uguali in tempi uguali \\
  v(t) = v_0 + a\,t                    & La velocità cresce linearmente nel tempo \\
  x(t) = x_0 + v_0 t + \tfrac12 a t^2  & Legge oraria del moto uniformemente accelerato \\
\end{formulario}
```

- **`mappa`** funziona come `schema`, con nodi più grandi e più distanziati.
- **`formulario`** è una tabella a due colonne. La prima è già in modo matematico: si scrive la formula senza `$`. La seconda è testo normale.

## Matematica e unità di misura

Sono caricati `amsmath`, `amssymb`, `mathtools`, `physics`, `siunitx` e `cancel`.

```latex
\dv{x}{t}            % derivata dx/dt
\dv[2]{x}{t}         % derivata seconda
\pdv{f}{x}           % derivata parziale
\vb{v}               % vettore in grassetto
\abs{x}  \norm{\vb{v}}
\cancel{m}           % termine semplificato, barrato

\qty{9,81}{\metre\per\second\squared}   % 9,81 m/s²
\num{1,5e3}                             % 1,5 · 10³
\unit{\newton\metre}                    % N m
```

`siunitx` è impostato con la **virgola decimale** e le unità composte con la barra (m/s², non m s⁻²).

## Impaginazione automatica

Lo stile evita da solo le situazioni più fastidiose, senza comandi manuali:

- **Capitoli** sempre a pagina nuova, con «Capitolo N» in maiuscoletto e filetto colorato.
- **Sezioni** spostate a pagina nuova se restano meno di circa 12 righe; **sottosezioni** se ne restano meno di 6. Niente titoli isolati in fondo alla pagina.
- **Niente righe vedove o orfane**; le pagine non vengono stirate (`\raggedbottom`).
- **Numerazione** fino alle sottosezioni; le `\subsubsection` sono senza numero, in grassetto corsivo.
- **Intestazione**: titolo del capitolo a sinistra, nome della materia a destra; numero di pagina centrato in basso.
- Pagina A4, margini di 2,7 cm, senza rientro di paragrafo.

Di conseguenza non servono `\newpage` o `\vspace` manuali prima dei titoli: se una sezione salta a pagina nuova lasciando spazio bianco, è il comportamento voluto.

### Disattivare il salto pagina: `\nonewpage`

Se preferisci che le sezioni proseguano sulla stessa pagina, senza lo spazio bianco in fondo, usa `\nonewpage`. Il salto resta attivo di default: va disattivato esplicitamente.

| Comando | Effetto |
|---|---|
| `\nonewpage` | Sezioni, sottosezioni e `\lezione` non saltano più a pagina nuova; `disegno` e `schema` non riservano più spazio extra |
| `\sinewpage` | Ripristina il comportamento di default |

```latex
\usepackage{../appunti-base}
\input{../palette-fisica}
\nonewpage                    % nel preambolo: vale per tutto il documento
```

Si possono usare anche nel testo, e valgono da quel punto in poi:

```latex
\nonewpage
\section{Sezione breve}      % prosegue sulla stessa pagina
...
\sinewpage
\section{Sezione lunga}      % di nuovo a pagina nuova se resta poco spazio
```

Cosa **non** cambia con `\nonewpage`:

- i capitoli iniziano sempre a pagina nuova, e così `\riepilogo`;
- un titolo non resta mai da solo in fondo alla pagina, staccato dal testo che lo segue;
- i box continuano a non spezzarsi tra due pagine (per quelli lunghi c'è l'opzione `lungo`);
- un disegno è un blocco unico: se non entra nello spazio rimasto va comunque alla pagina successiva.

## Cose a cui fare attenzione

- **La numerazione delle definizioni può saltare.** `definizione` e `teorema` condividono lo stesso contatore, ma il numero compare solo nelle definizioni. Dopo «Definizione 1.1» e un teorema, la definizione successiva è «1.3». Gli esempi non sono numerati.
- **Graffe del titolo obbligatorie** in `definizione`, `teorema`, `esempio`: dimenticarle fa sì che la prima lettera del testo venga presa come titolo.
- **Niente `$` dentro `formula`** e nella prima colonna di `formulario`.
- **`\evid` su testi lunghi** va a capo normalmente, ma al suo interno funzionano solo testo e i comandi `\chiave`, `\implica`, `\incerto`, `\aggiunto`. Per formule in mezzo al testo evidenziato, chiudi `\evid`, scrivi la formula con `$\evid{...}$` e riapri.
- **`\input{../palette-...}` dopo `\usepackage{../appunti-base}`**, non prima.
- **I percorsi `../` valgono per un `.tex` dentro una cartella di argomento.** Se un file sta nella root accanto allo stile, si tolgono; se sta due livelli più in basso, diventano `../../`.
- **`\nonewpage` non toglie i `\newpage` scritti a mano**: disattiva solo il salto automatico prima dei titoli.
- **`esempio` e `nota` sono sia nomi di colori sia nomi di ambienti**: non c'è conflitto, ma non ridefinire quei colori con altri significati.