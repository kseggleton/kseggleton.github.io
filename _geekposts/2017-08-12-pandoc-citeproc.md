---
layout: post
title: Bookends, Pandoc and institutional authors
date: 2017-08-12
---

I write in Markdown and use [Pandoc](http://pandoc.org) to output Word/PDF documents. A Pandoc filter, called [pandoc-citeproc](https://github.com/jgm/pandoc-citeproc/blob/master/man/pandoc-citeproc.1.md), can scan the Markdown file and replace the reference placeholders with the correct citation based on a BibTex file. [Bookends](http://sonnysoftware.com) can export BibTex files and this is what I have been doing. However, one of the issues that has prevented me from completely going over to pandoc-citeproc has been that institutional authors are abbreviated in the output. For example *Ministry of Health* would look like *Ministry, OH* in the Pandoc processed document. I have struggled with a way to avoid manually correcting the BibTex file (can be done by enclosing institutional authors with curly brackets) and have stumbled upon the following hack in Bookends (which I use to export the BibTex file). The hack is to enter the institutional author in a user field (I have used user8) and then to create a custom format that is based on *BibTex.fmt*. (I have unimaginatively called mine *BibTex modified.fmt*). Within the format manager make the following changes to the author field in all reference types:

    author = $`{`{~{~u8~}~^a}`}`, $

What this does is output the user8 field first surrounded by curly brackets. However, if this field is empty then the author field will be used instead. The result is that pandoc-citeproc will correctly format the institutional authors rather than abbreviating them (so long as you enter them in the user8 field). This means that all my Word/PDF documents are correctly referenced and I avoid having to scan them in Bookends to generate a bibliography.