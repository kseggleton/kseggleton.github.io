---
layout: post
title: Javascript bookmarklet and PDF Expert
---

[PDF Expert 6](https://pdfexpert.com/ios) came out today and I discovered that it had a simple URL scheme for opening PDFs. Adding `pdfe` to the beginning of a web URL will launch PDF Expert and the PDF is imported.

There is a simple trick to automate this process and that is to create a Javascript bookmarklet. I keep the bookmarklet in the tool bar of iCab and when I want to import a PDF I will just select the bookmarklet. The bookmarklet is: `javascript:location.href='pdfe'+location.href`