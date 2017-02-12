---
layout: post  
title: Sending AirMail email to DevonThink  
date: 2017-02-12
---  
I spend a lot of my day managing email and so selecting a good email programme is therefore very important. Over the years I have mostly used Apple Mail but more recently I have been drawn to the power of [AirMail](http://airmailapp.com) by Bloop Software. AirMail ticks most of the buttons of what I want in an email programme, such as reminders, deferring email, markdown composing and integration with a range of other programmes. But one thing that I really wanted to be able to do was to integrate Airmail with DevonThink. My preferred workflow is to triage my email, decide on the course of action required, i.e. reply immediately, defer until later, trash or place in my task list for action. Part of the workflow also involves placing a copy of the email and any associated attachments in my project management structure managed by DevonThink. My project structure, which is created from a DevonThink template, typically looks like this: 

![Project structure in DevonThink]({{ site.url }}/images/projectstructure.png)

The failing of AirMail has been the difficulty in automating the export of email to DevonThink. However, AirMail has some rudimentary AppleScript support and the following scripts now enable me to export email and attachments to DevonThink. I have attached the applescripts to an Alfred workflow and call the workflow with a keyboard shortcut thus automating the process with a minimum of keystrokes.

**Applescript to send mail message from AirMail to DevonThink**  
{% raw %}
{*
name:           	Save mail message
description:	Use as a script to save mail message from Airmail to DevonThink
author:	Kyle Eggleton www.eggleton.co.nz
*)



-- Get contents of the email
tell application "Airmail 3"
	set x to the count of the selected message
	
	if x is 0 then
		display alert "Airmail 3" message "There are no messages selected" as warning
		return
	end if
	
	
	set {theSender, theSubject, theSource} to {the sender, the subject, the htmlContent} of the selected message
	set theURL to the selectedMessageUrl
	
	-- add header information
	set theBody to "To: 
Subject: " & theSubject & "
From: " & theSender & "
Date: 
MIME-Version: 1.0
Content-Type: text/html; charset=\"iso-8859-1\"


" & theSource
end tell

-- Select the group to send the email to DevonThink
tell application id "DNtp"
	if not (exists current database) then display alert "Airmail 3" message "No database is in use in DevonThink" as warning
	set theGroup to display group selector
	
	-- Import the email into DevonThink as record
	create record with {name:theSubject & ".eml", source:theBody, type:unknown, URL:theURL} in theGroup
end tell
{% endraw %}

**AppleScript to send attachments from AirMail to DevonThink**  

{% raw %}
(*
name:           	Save attachments
description:	Use as a script in Airmail rules to save attachments to the selected folder.
author:	Kyle Eggleton www.eggleton.co.nz
*)



tell application "Airmail 3"
	
	-- Get the count of the number of attachments
	set x to the count of mail attachment of the selected message
	
	-- End script if the message has no attachments
	if x is 0 then
		display alert "Airmail 3" message "There are no attachments in this message" as warning
		return
	end if
	
	-- Select the group to send attachments to in DevonThink
	tell application id "DNtp"
		if not (exists current database) then display alert "Airmail 3" message "No database is in use in DevonThink" as warning
		set theGroup to display group selector
	end tell
	
	-- Import the attachments into DevonThink
	set i to 1
	repeat until i = x + 1
		set theAttach to filename of mail attachment index i of the selected message
		set theFile to POSIX path of theAttach
		tell application id "DNtp"
			import theFile to theGroup
		end tell
		set i to i + 1
	end repeat
end tell
{% endraw %}
