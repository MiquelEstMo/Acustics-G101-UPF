# 📚 Treball en grup (LaTeX + Git)

Plantilla basada en [tau-class](https://github.com/MemoJimenez/Tau-class) (CC BY 4.0), adaptada a **una columna**, **sense abstract**, amb **portada** i color **burdeus**.

## 📑 Índex

1. Estructura del projecte
2. Com treballem amb una secció per fitxer
3. Compilar
4. Rutina amb Git
5. Guia ràpida de LaTeX
6. Normes del grup
7. Problemes freqüents

---

## 1. 🗂️ Estructura del projecte

```
trabajo/
├── main.tex              <- document principal (preàmbul + \input de cada secció)
├── portada.tex           <- portada
├── bibliografia.bib      <- totes les referències
├── secciones/
│   ├── 01_introduccion.tex
│   ├── 02_desarrollo.tex
│   ├── 03_resultados.tex
│   └── 04_conclusiones.tex
├── figures/              <- imatges (PDF, PNG, JPG)
├── tau-class/            <- la classe (NO tocar)
├── logo.png              <- (opcional) logo per a la portada
└── .gitignore
```

- `main.tex` només conté la configuració i els `\input`. **Ningú escriu contingut aquí.**
- Cada fitxer de `secciones/` conté **només el contingut**: comença a `\section{...}` i no porta `\documentclass` ni `\begin{document}`.
- La carpeta `tau-class/` és la plantilla. No la modifiqueu sense avisar el grup.

---

## 2. 🧩 Com treballem amb una secció per fitxer

**Idea:** cada persona té assignats un o diversos fitxers de `seccions/` i edita només aquests. Git només dona conflictes quan dues persones toquen **les mateixes línies** del **mateix fitxer**, així que repartint fitxers gairebé no n'hi haurà.

### Afegir una secció nova

1. Crea `seccions/05_nom.tex` amb aquest contingut:
   ```latex
   \section{Títol de la secció}
   \label{sec:nom}

   Text...
   ```
2. Afegeix `\input{seccions/05_nom}` a `main.tex` al lloc que toqui (aviseu el grup, perquè és un canvi en un fitxer compartit).

### Si una secció és molt gran

Es pot dividir en subfitxers: `seccions/02_desarrollo.tex` pot contenir `\input{seccions/02a_teoria}` i `\input{seccions/02b_modelo}`, i així dues persones treballen a la mateixa secció sense trepitjar-se.

### ⚠️ Fitxers compartits (els únics amb risc de conflicte)

- `main.tex`: paquets, títol, autors. Canvis petits i pujats de seguida.
- `bibliografia.bib`: afegiu les vostres referències **al final** del fitxer.

---

## 3. ⚙️ Compilar

**A VS Code (LaTeX Workshop):** botó verd *Build LaTeX project* o `Ctrl+Alt+B`. Compileu sempre `main.tex`, encara que estigueu editant una secció. Per obrir el PDF: `Ctrl+Alt+V`.

**Des del terminal:**

```
latexmk -pdf main.tex
```

Es compila amb `pdflatex` i la bibliografia amb `biber`. Si falta algun paquet a TinyTeX: `tlmgr install nom_del_paquet`.

---

## 5. ✍️ Guia ràpida de LaTeX

### 5.1 Text

```latex
Text normal, \textbf{negreta}, \textit{cursiva} i \emph{èmfasi}.
Codi en línia: \inlinecode{git pull}.
```

- Un paràgraf nou es fa amb **una línia en blanc**.
- Caràcters especials: escriviu `\%`, `\&`, `\$`, `\#`, `\_` (amb barra al davant).
- Cometes: ``` ``així'' ``` i no `"així"`.
- Comentaris: tot el que va darrere de `%` s'ignora.
- Espai que no es parteix entre línies: `10~km`, `Fig.~3`.
- `\inlinecode{}` no funciona dins de `\footnote{}`, títols de secció ni altres comandes. Allà feu servir `\texttt{}`.

### 5.2 Títols i apartats

```latex
\section{Títol}
\subsection{Subapartat}
\subsubsection{Sub-subapartat}
\paragraph{Títol en línia.} Text que segueix a la mateixa línia.
```

Per a un apartat **sense numerar**: `\section*{Títol}`. Els títols de `\section` surten automàticament en majúscules.

**Lletra capital** (opcional, només al primer paràgraf d'una secció):

```latex
\taustart{A}quest és el primer paràgraf...
```

### 5.3 Llistes

```latex
\begin{itemize}
    \item Element u
    \item Element dos
\end{itemize}

\begin{enumerate}
    \item Primer
    \item Segon
\end{enumerate}
```

### 5.4 🖼️ Imatges

Poseu la imatge a `figures/` (millor en PDF o PNG, sense espais ni accents al nom).

```latex
\begin{figure}[H]
    \centering
    \includegraphics[width=0.7\textwidth]{figures/esquema.pdf}
    \caption{Esquema del sistema.}
    \label{fig:des-esquema}
\end{figure}
```

- `[H]` col·loca la figura **just on l'escriviu**.
- `width=0.7\textwidth` és el 70 % de l'amplada de la pàgina.
- Referenciar: `\figref{fig:des-esquema}` → "Fig. 1" (amb enllaç al PDF).

### 5.5 📊 Taules

```latex
\begin{table}[H]
    \caption{Dades d'exemple}
    \label{tab:res-datos}
    \centering
    \begin{tabular}{lcr}
    \toprule
    \textbf{Nom} & \textbf{Tipus} & \textbf{Valor} \\
    \midrule
    Alfa & A & 1,5 \\
    Beta & B & 2,7 \\
    \bottomrule
    \end{tabular}
    \tabletext{Nota: font de les dades.}
\end{table}
```

- `lcr` = columnes alineades a l'esquerra (**l**eft), al centre (**c**enter) i a la dreta (**r**ight). Una lletra per columna.
- Les columnes se separen amb `&` i les files acaben amb `\\`.
- Feu servir `\toprule`, `\midrule` i `\bottomrule` (estil `booktabs`), **sense línies verticals**.
- `\tabletext{...}` afegeix una nota petita sota la taula (opcional).
- Referenciar: `\tabref{tab:res-datos}` → "Tabla 1".
- Per a una cel·la amb text llarg feu servir una columna `p{4cm}` en lloc de `l`.

### 5.6 💡 Blocs destacats (note, info, tauenv)

```latex
\begin{note}
    Això és una nota.
\end{note}

\begin{info}
    Això és un bloc d'informació.
\end{info}

\begin{tauenv}[frametitle=Títol propi]
    Bloc amb el títol que vulgueu.
\end{tauenv}
```

`note` i `info` posen el seu títol sols ("Nota", "Información"). Amb `tauenv` el trieu vosaltres.

### 5.7 ➗ Equacions

```latex
En línia: $a^2 + b^2 = c^2$.

Numerada i a la seva pròpia línia:
\begin{equation}\label{eq:des-energia}
    E = mc^2
\end{equation}

Com es veu a l'\equref{eq:des-energia}...
```

- Referenciar: `\equref{eq:des-energia}` → "Ecuación 1".
- Sense numerar: `\[ E = mc^2 \]`.
- Per a entorns com `align` o `cases` cal `\usepackage{amsmath}` a `main.tex` (aviseu el grup).

### 5.8 💻 Codi

```latex
\begin{code}
\begin{lstlisting}[language=Python]
def saluda(nom):
    print("Hola", nom)
\end{lstlisting}
\caption{Exemple de codi.}
\label{lst:des-saluda}
\end{code}

Vegeu el \coderef{lst:des-saluda}.
```

Es fa servir `listings`. El paquet `minted` està desactivat a propòsit, perquè exigeix instal·lacions extra i `--shell-escape`.

### 5.9 Referències creuades

| Què | S'etiqueta amb | Es cita amb |
|---|---|---|
| Figura | `\label{fig:...}` | `\figref{fig:...}` |
| Taula | `\label{tab:...}` | `\tabref{tab:...}` |
| Equació | `\label{eq:...}` | `\equref{eq:...}` |
| Codi | `\label{lst:...}` | `\coderef{lst:...}` |
| Secció | `\label{sec:...}` | `\ref{sec:...}` |

**⚠️ Important:** el `\label` va **després** del `\caption` (o del `\section`).

**Prefixos d'etiqueta per secció** (perquè dues persones no repeteixin nom):

| Secció | Prefix | Exemple |
|---|---|---|
| Introducció | `intro` | `\label{fig:intro-mapa}` |
| Desenvolupament | `des` | `\label{tab:des-datos}` |
| Resultats | `res` | `\label{fig:res-grafica}` |
| Conclusions | `conc` | `\label{tab:conc-resumen}` |

### 5.10 📖 Bibliografia i cites

1. Afegiu la referència **al final** de `bibliografia.bib`:
   ```bibtex
   @book{garcia2024,
     author    = {Ana García},
     title     = {Títol del llibre},
     publisher = {Editorial},
     year      = {2024}
   }
   ```
   Clau = `cognomAny` (`garcia2024`). Si dues coincideixen, afegiu una lletra (`garcia2024b`).
2. Citeu al text: `\cite{garcia2024}` → [1]. Diverses alhora: `\cite{garcia2024,lopez2023}`.
3. La llista de referències es genera sola al final del document. Només hi apareixen les citades.
4. Per aconseguir el BibTeX d'un article o web: Google Scholar (*Citar → BibTeX*) o [scribbr](https://www.scribbr.es/citar/generador/).

### 5.11 Enllaços i notes al peu

```latex
\href{https://www.overleaf.com}{text de l'enllaç}
\url{https://github.com}
Text amb nota.\footnote{Text de la nota al peu.}
```

### 5.12 Salts de pàgina i espais

```latex
\newpage        % salta de pàgina
\vspace{1cm}    % espai vertical
```

Feu-los servir el menys possible. És millor deixar que LaTeX reparteixi el contingut sol.

---
