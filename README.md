
# MFLatex

This repository is my sandbox to making my life easier with latex. 

It contains multiple LaTeX packages with documentation. This document is a short summary.

**Author**: Morgan Fouesneau


-------------

## mf - Personal common macros and aliases.

Commands are defined using `providecommand` so that it will neither 
redefine existing macros nor raise an error. Hence the order of loading
packages matters.


### Content

Each section name has the associated option name given within [], apart from `common` that is always set.

* **common**:
some alias to commonly used/internal functions 

* **probabilities**:  [`probs`]
personal notations when writing probabilities

* **figure convertions**: [`fig`]
Handles automated conversion for appropriate format

* **astro units**  [`astro`]:
Common units in astronomy (works in math/text mode)

* **journal abbreviations** [`journals`]
Common Journal abbreviations in astronomy

* **names** [`names`]
lazyness at its maximum for proper spelling of names



### Example Usage


```latex
\usepackage[math, fig]{mf}
```

### Requirements
* ifpdf
* amsmath
* color


-------------
## pgm - probabilistic graphical models

A graphical model is a probabilistic model for which a graph denotes the
conditional dependence structure between random variables. They are commonly
used in probability theory, (Bayesian) statistics, and machine learning.

### Commands

* `\plate`: A plate encapsulates (independent) processes in the model.

   4 parameters: e.g.: `\plate{galaxy}{(40,0)}{(570,240)}`
    * `#1`: plate's label that will be display in the top left corner 
    * `#2`: top-left corner of the plate rectangle
    * `#3`: bottom-right corner of the plate rectangle


* `\quantity`: Add a quantity to the model. A quantity is the representation of a random variable in a PGM. If the quantity is observed or fixed, refer to `\observedquantity` and `\fixedquantity`, respectively.

   4 parameters: e.g.: `\quantity{$\tau$}{(80, 60)}{tau}`
    * `#1`: any option to pass to \draw (default empty)
    * `#2`: text
    * `#3`: center position
    * `#4`: node name
    
  `TODO`: make option fixed and observed instead of independent commands

*  `\fixedquantity`

  Add a fixed quantity to the model. A fixed quantity is assumed to be known, i.e., not a random variable.

  4 parameters: e.g.: `\fixedquantity{$\tau$}{(80, 60)}{tau}`
  * `#1`: any option to pass to \draw (default empty)
  * `#2`: text
  * `#3`: center position
  * `#4`: node name


* `\observedquantity`

  Add an observed quantity to the model. An observed quantity is a measurement of a random variable, i.e., an observable.

  4 parameters: e.g.: `\observedquantity{$\tau$}{(80, 60)}{tau}`
  * `#1`: any option to pass to `\draw` (default empty)
  * `#2`: text
  * `#3`: center position
  * `#4`: node name


* `\relation`

  a conditional relation between two quantities.

  2 parameters: e.g. `\relation{tau}{ts}`
  * `#1`: the conditional variable node's name 
  * `#2`: the conditioned variable node's name


###Global variables


#### colors 
use `\definecolor` to redefine


* `plate-draw-color`: default `{rgb}{.3, .3, .3}`
    
   plate edge color


* `plate-fill-color`: default `{rgb}{0.95, 0.95, 0.95}`

   plate fill color (`\plateOpacity` set to 0.5)


* `quantity-draw-color`: default `{rgb}{.0, .0, .0}`

   quantity edge color (fill color in case of fixed quantity)


* `quantity-fill-color`: default `{rgb}{1., 1., 1.}`

   quantity fill color


* `relation-draw-color`: default `{rgb}{.0, .0, .0}`

   color to draw a relation


#### shapes
use `\def` to redefine

* `\plateLineWidth`: default `1pt`

  plate edge width


* `\plateRoundedCorner`: default `0cm`

  radius of the rounding box of a plate 


* `\quantityLineWidth`: default `1pt`

  edge width of the quantities


* `\quantityCircleSize`: default `20pt`

  quantity node radius


* `\relationLineWidth`: default `1pt`
  
  relation line width


* `\relationHeadScale`: default 2
  
  relation arrow head scaling


### Requires

* tikz
* color

### Example Usage

```latex
%example from Fouesneau et al. 2014, in prep
\documentclass[12pt]{article}
\usepackage[utf8]{inputenc}
\usepackage{graphicalmodel}

\usepackage[active,tightpage]{preview}
\PreviewEnvironment{tikzpicture}
%make all math nice looking
\everymath{\displaystyle}

\begin{document}
% outer sep: global separation between elements
% inner sep: global separation with embeded elements
% x: x-length units
% y: y-length units
\begin{tikzpicture}[y=1pt,x=1.pt, xscale=1., yscale=-1, 
      inner sep=0pt, outer sep=0pt]
\begin{scope}

%host influence
\plate{galaxy}{(40,-10)}{(430,240)}
\quantity{$\tau$}{(80, 60)}{tau}
\quantity{$\tilde{t}$}{(160, 110)}{ts}
\quantity{$\frac{\rm dM}{\rm dt}$}{(160,200)}{dM/dt}
\quantity{$\gamma_d$}{(80,150)}{gd}
\quantity{$\gamma_e$}{(0,200)}{ge}

\relation{tau}{ts}
\relation{gd}{dM/dt}
\relation{ge}{dM/dt}
\relation{ts}{dM/dt}

% birth rate
\plate{birth rate}{(200, 10)}{(410, 190)}
\quantity{CIMF}{(300,60)}{cimf}
\quantity{CFH}{(370,60)}{cfh}
\quantity{M$_{i}$}{(240,150)}{Mi}
\quantity{t$_{\rm{age}}$}{(310,150)}{tage}

\relation{cimf}{Mi}
\relation{cfh}{tage}

%other
\quantity{M$_{\rm{pred}}$}{(240,280)}{Mpred}
\quantity{N$_{\rm{pred}}$}{(340, 280)}{Npred}
%\quantity{N$_{\rm{cl}}$}{(250, 400)}{Ncl}

\relation{Mpred}{Npred}
%\relation{Ncl}{Npred}

\relation{Mi}{Mpred}
\relation{tage}{Mpred}
\relation{dM/dt}{Mpred}

\end{scope}

\end{tikzpicture}
\end{document}

```

