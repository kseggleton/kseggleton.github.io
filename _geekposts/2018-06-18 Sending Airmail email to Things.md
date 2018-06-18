---
layout: post
title: Sending Airmail email to Things
---

While [Airmail](http://airmailapp.com) can send an email as a task to [Things](https://culturedcode.com/things/) it is, in my opinion, missing an important part and that is the body of the email. Having the body of the email in the task note makes task management more effective. In order to achieve this the following Applescript will populate the Things quick entry panel with the first 1000 characters of the email and place a link back to the original email in the notes. One key component is trimming the characters (in order to not overwhelm the quick entry panel with text). While there might be an Applescript solution for this I instead simply call the following [TextSoap](https://www.unmarked.com/textsoap/) action.

![Trim text length action](/images/trim_text.png)

The Applescript is here:

```
(*
name:           	Save mail message
description:	Use as a script to save mail message from Airmail to Things as a task
author:		Kyle Eggleton www.eggleton.co.nz
*)



-- Get contents of the email
tell application "Airmail 3"
	set x to the count of the selected message
	
	if x is 0 then
		display alert "Airmail 3" message "There are no messages selected" as warning
		return
	end if
	
	
	set theSource to the content of the selected message
	set theURL to the selectedMessageUrl
end tell

tell application "textsoap8Agent"
	set theCleanText to cleanText theSource with "Extract Text from HTML Source"
	set theText to cleanText theCleanText with "Trim text length"
end tell

-- Fill out quick entry panel in Things 3

tell application "Things3"
	show quick entry panel with properties {name:"Enter title", notes:theURL & return & return & theText}
end tell
```