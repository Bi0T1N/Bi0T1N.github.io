---
layout: default
title:  "Colored source code output with LaTeX and Pygments"
categories: [latex, python, source code, minted, colorful, Pygments]
---

# Colored source code output with LaTeX and Pygments
## Introduction
If you want to include source code in your LaTeX document, there are several ways.  
One way is to use the `listings` environment but the default output doesn't look very nice.

```LaTeX
\usepackage{listings}
...
\begin{lstlisting}[language=Python]
	import tensorflow as tf
	a = 1
	b = 2
	r = tf.add(a, b, name='Add')
	print(r)
\end{lstlisting}
```

![LaTeX listings output](/images/listing_output.png)

Another way is to use the `minted` environment which uses the Python Pygments package which is a generic syntax highlighter supporting a variety of [languages](https://pygments.org/languages/).

```LaTeX
\usepackage{minted}
...
\begin{minted}{python}
	import tensorflow as tf
	a = 1
	b = 2
	r = tf.add(a, b, name='Add')
	print(r)
\end{minted}
```

![LaTeX minted output](/images/minted_output.png)

## Installation steps (Linux)
1. Install `minted` package: `sudo apt-get install texlive-latex-extra`
2. Install the Pygments program: `sudo apt-get install python-pygments`
3. Append `-shell-escape` in the settings for PdfLaTeX in [TeXstudio](https://github.com/texstudio-org/texstudio)
	* ![TeXstudio PdfLaTeX settings](/images/TeXstudio_settings.png)
4. Use the `minted` environment with your [preferred source code style](https://www.overleaf.com/learn/latex/Code_Highlighting_with_minted#Reference_guide)
4. Enjoy the nicely colored source code in your document!

## Sources
[Pygments](https://pygments.org/)  
[How to install minted in Ubuntu](https://tex.stackexchange.com/a/40101)
