#LaTeX #documents

# Download $\LaTeX$
- [compiler]([Home](https://miktex.org/))
- [Text editor]([Texmaker (free cross-platform latex editor)](https://www.xm1math.net/texmaker/download.html))
# Creating a document
## Preamble
The section of the document before the `\begin{document}` used to specify type and packages 

To begin instantiate the document by specifying the type of document you are creating using:
```LaTex
\documentclass[*font size*]{*insert type*}
```

To insert comments in the code use `%` :
```LaTeX
 % Insert comment here
```
## Types of documents
1. article - common pdfs
2. beamer - slide/presentations
3. book
4. exam
5. report

# Imports
For any external features to be imported in the document the need to be imported:

```LaTex
\usepackage{graphicx} % Required for inserting images
```

# Starting a document
To specify the scope of the document
```LaTex
\begin{document}
hello this is \LaTeX\ boy
\end{document}
```

# Document formatting
## Indentation

To remove or specify document indentation in the `preamble` :
```latex
\parindent 0px % 0 pixels
```
## New line
To insert go to a new line on the document 
### Option 1
Use `\\` to signal new line you can also specify the line spacing using `[Xpt]`
```latex
hello\\[6pt]
This is a new line
```

### Option 2 
skip a line on your LaTeX code
```latex
\begin{document}
hello 

This is a new line
\end{document}
```

## Math mode

Useful packages
```latex
\usepackage{amsfonts, amssymb, amsmath}
```
### Inline math
To write math expressions within a sentence encase it `$...$` :
```latex
find $f(x) = x^2 +3$ 
```

To ensure the equations are in the same line in case it wraps to the next put it inside of `${...fx...}$`.  

### Displayed math
To have the equations occupy their own lines eg. on textbooks encase it in `$$...$$`

## Brackets and symbols
In math mode:
- square brackets $[ ...]$
- curly braces $\{...\}$
- angular brackets $\langle ...\rangle$
- dollar sign $\$10.5$
- fractions $\frac{1}{2+3}$
- To in case fraction in brackets $\left(\frac{1}{2+3}\right)$
- Fraction with bracket on onesie $\left. \frac{1}{2+3}\right|$ 
- Subscript `$_{blah}$` 

```latex
find ${f(x) = x^2 +3}$\\[6pt]

Root of the problem $$g(f(x)) = mass*C^2 = E$$ \\

$\left(\frac{1}{2+3}\right)$\\

$$ \left\langle\frac{dy}{dx}\right\rangle $$\\
$$ \left.\frac{dy}{dx}\right|_{i donno} $$
```

# Tables
To create a table you need to specify the alignment and number of row using `r/c/l` 

To draw the vertical line  use `|` and for horizontal line `\\ \hline` 

Each column is separated by `&`

```latex
% 4 column 2 row table with right align
\begin{tabular}{|r|r|r|r|}
\hline
$x$ & 2 & 3 & 4\\ \hline
$f(x)$ & 4 & 5 & 6\\ \hline
\end{tabular}
```

For table with more space:
- To ensure it stays where you define it in code lock it in place using `[H]`
- To define the padding in the cells `\def\arraystretch{num}`

```latex
\usepackage{float}

% 4 column 2 row table with right align
\begin{table}[H]
\def\arraystretch{1.3}
\begin{tabular}{|r|r|r|r|}
\hline
$x$ & 2 & 3 & 4\\ \hline
$f(x)$ & 4 & 5 & 6\\ \hline
\end{tabular}
\end{table}
```

- To center the table
```latex
\begin{table}[H]
\centering
```

- Add a caption to the table either above `before \begin{tabular}` or below `after \end{tabular}`
```latex
\end{tabular}
\caption{x values for f(x)}
\end{table}
```


