---
layout: post
title: Sending text to Marked with Alfred
---

I have been playing around with [Drafts](https://getdrafts.com/mac/) for Mac for a little while and it has now taken over all my note taking function. Previously my notes were stored on Dropbox and accessed through a variety of text editors. One of the issues with the Mac version of Drafts, so far, is the lack of actions. However, [Alfred](https://www.alfredapp.com) can easily be used on text selections to automate a number of tasks. One automation is sending the draft to [Marked](https://marked2app.com) for a nice preview (with a variety of themes based on CSS) and the ability to export to a range of formats. In Alfred create a new workflow with a trigger that takes the selected text. Pass the contents of this to the following Bash script:

```
QUERY=$1
TMPFILE=`mktemp -t marked2-preview`
echo "$QUERY" > $TMPFILE
open -a "Marked 2.app" $TMPFILE
osascript -e 'tell application "Marked 2" to activate'
```

A copy of the workflow is [here](/attachments/textToMarked.alfredworkflow)