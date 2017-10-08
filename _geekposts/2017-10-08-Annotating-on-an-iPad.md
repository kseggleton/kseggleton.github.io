---
layout: post
title: Annotating on an iPad
---
The iPad is ideally suited for reading PDFs and making highlights and annotations. With the addition of the Apple Pencil highlighting PDFs got even easier and is now my preferred way of reading articles. However, I have not found an ideal solution for getting annotations into DevonThink and rely upon a series of hacks to export my highlights. There are three strategies that I use:

## PDF Expert annotations to Markdown to DTTG

[PDF Expert](https://pdfexpert.com/ios) has the ability to export a copy of annotations. Each annotation is prefixed by the type of annotation e.g. highlight or note. The annotations are exported as an HTML file. While it is possible to export this directly to DTTG it is not my preferred option as I like to use Markdown. This relatively simple [Workflow](https://workflow.is/workflows/91668c70e07841caaf142638aa35d3e5) can reformat the HTML file into Markdown and then send the resulting file to DTTG. It does two additional things: 

1. The Workflow will strip out the text 'highlight' and replace with a Markdown quote marker (i.e. >). 
2. In addition all text beginning with 'note' will have the 'note' word stripped.  

## PDF Expert annotations split into individual Markdown files to DTTG 

While I generally prefer to have all my annotations in one file some people prefer to have the annotations split up and an individual file for each quote. The reasoning behind this is that DevonThink's AI can find similar items more accurately using the 'Magic Hat' feature. In this more complicated [Workflow](https://workflow.is/workflows/ce317616dba342f3bd6574339a8f7289) each annotation is split up and saved as individual files (named according to the first 35 characters of the quote) in a group named after the PDF.

## PDF Expert annotations to OPML

For more complex readings I often like to create an OmniOutliner file and have the annotations in one column, paraphrased notes underneath the quotes and my ideas in a second column. I use a template with different colours to indicate whether the text relates to background/results, methodology, conclusion or references.

![Annotations template example](/images/omnioutliner.png)

The [Workflow](https://workflow.is/workflows/ed0afa6b2f144208bf41b7b47ea8af54) to export the annotations file from PDF Expert to OmniOutliner is a simple variation of the first Workflow that I outlined. It simply strips superfluous text such as 'notes' and 'highlights' and opens an 'Open in...' dialogue box where I select OmniOutliner. I then apply my annotations template and create notes, ideas then arrange the text into logical groupings. I finally export the OmniOutliner file to DTTG. At the moment DTTG can not do a quick preview of OmniOutliner files, but on the desktop these files can be viewed in DevonThink and the discloure triangles opened or closed.