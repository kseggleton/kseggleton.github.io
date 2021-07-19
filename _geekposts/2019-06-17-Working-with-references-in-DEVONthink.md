---
layout: post  
title: Working with references in DEVONthink
date: 2019-06-17
---

My general workflow involves importing PDFs into DEVONthink 3, renaming them based on an *Author, Date* format, applying tags and labels. Bookends is the referencing program that I use to generate citations and bibliographies.

## Getting a PDF into Bookends

For those PDFs that I think I want to reference in the future I will export them to Bookends. The attachments folder for Bookends is the same folder that DEVONthink indexes. In order to export a reference to Bookends I use the following script:

```
-- Exports record from DEVONthink to Bookends and creates link
-- Kyle Eggleton April 2019

tell application id "DNtp"
	
	-- Get the selection in DEVONThink
	set theRecord to the content record of think window 1
	if theRecord is {} then error "Nothing selected in DEVONThink Pro"
	set theURL to reference URL of theRecord
	set theFile to filename of theRecord
	
	set theAuthorDate to name of theRecord as string
	
	-- Get rid of the date from the record name
	set AppleScript's text item delimiters to {" "}
	set delimitedList to every text item of theAuthorDate as string
	set theAuthor to text item 1 of delimitedList & " " & text item 2 of delimitedList
	set theDate to last text item of delimitedList
	
	
	-- Extract the title from the meta data
	set theMD to meta data of theRecord
	set theTitle to kMDItemTitle of theMD
end tell

-- Create new publication in Bookends and link to record in DEVONthink (using user4)

tell application "Bookends"
	tell front library window
		make new publication item with properties {title:theTitle, authors:theAuthor, publication date string:theDate, user4:theURL, attachments:theFile}
		set theId to id of last publication item
		activate "Bookends"
	end tell
end tell

-- Set URL to DEVONThink in Bookends

tell application id "DNtp"
	set the URL of theRecord to ("bookends://sonnysoftware.com/" & theId) as text
end tell

--Activate Bookends

tell application "Bookends"
	activate
end tell
```

The script uses the *Author, Date* naming convention that I use to generate the author of the reference in Bookends and the date of publication. I will then complete the reference details using the auto-complete feature in Bookends.

## Updating the DEVONthink record


Once the PDF is in Bookends and the reference details are complete then I like to have basic reference details in DEVONthink. I have created some custom meta data fields that store this information, along with links to any annotations I might make or reading briefs.

![Custom meta data fields in DEVONthink]({{ site.url }}/images/custommetadata_dntp.png){:width="300px"}

For my purposes I use a script that links the selected Bookends reference to the selected DEVONthink record. **The script uses an external dependency - exiftool, that will need to be installed separately**. Exiftool changes the title and author fields of a PDF. One other point of note is that because I use pandoc-citeproc for my citations, the citations format is BibTex and stored in the *user1* field in Bookends. 

```
-- Script to link DEVONthink file and Bookends reference along with custom metadata
-- Kyle Eggleton April 2019

tell application "Bookends"
	tell front library window
		
		-- Get selected publication 
		
		set theRefs to selected publication items
		set theRefsNo to count of theRefs
		set theRef to first item of theRefs
		
		-- Error messages	
		
		if theRefsNo is greater than 1 then error "Select only one item"
		if theRefs is {} then error "Nothing selected in Bookends"
		
		-- Get properties of selected reference
		
		set theID to id of theRef
		set theCitation to user1 of theRef
		set theAbstract to abstract of theRef
		set theAbstract to abstract of theRef
		set theAuthor to authors of theRef
		set theTitle to title of theRef
		set theAttachments to attachments of theRef
		set theJournal to journal of theRef
		set theFormattedReference to format theRef using "APA 6th Edition Markdown.fmt" --Change to theformat in Bookends that you want
		
		-- Only set metadata fields for PDF if there is an attachment
		
		if theAttachments is equal to "" then error "There is no file attached to the Bookends reference"
		if theAttachments is not equal to "" then
			set theFile to the first attachment item of theRef
			set thePath to the path of theFile
			if theFile is "" then error "The file is missing from the Bookends reference"
			
			-- Set the metadata fields for PDF. Need to install exiftool (https://www.sno.phy.queensu.ca/~phil/exiftool/install.html)
			do shell script "/usr/local/bin/exiftool -title=" & quoted form of theTitle & " -author=" & quoted form of theAuthor & " -subject=" & quoted form of theJournal & " -overwrite_original " & quoted form of thePath
		end if
	end tell
end tell


-- Set URL to Bookends and custom metadata in DevonThink. Need to create custom meta data fields for Abstract, Citation and Reference

tell application id "DNtp"
	set theURL to ""
	set theRecord to the content record of think window 1
	if theRecord is {} then error "Nothing selected in DEVONThink Pro"
	set the URL of theRecord to ("bookends://sonnysoftware.com/" & theID) as text
	add custom meta data theAbstract for "abstract" to theRecord
	add custom meta data theCitation for "citation" to theRecord
	add custom meta data theFormattedReference for "reference" to theRecord
	set theURL to reference URL of theRecord
end tell

-- Set URL to DEVONThink in Bookends (using user4 field)

tell application "Bookends"
	tell front library window
		set user4 of publication item id theID to theURL
	end tell
	
end tell
```

## Getting annotations from Bookends

My annotations workflow varies a lot. If I am on my iMac I often use the Bookends annotation feature and create *Note Cards* with PDF highlights. I use the following script to create a Markdown annotations summary in the *Annotations* group in the DEVONthink database where the indexed PDF exists. I also creates a link between the indexed PDF and the Markdown annotation summary that is stored in a custom meta data field outlines above.

```
-- Script to creat Annotations summary from Bookends reference linked to indexed  DEVONthink file 
-- Kyle Eggleton June 2019

tell application "Bookends"
	tell front library window
		
		-- Get selected publication 
		set theRefs to selected publication items
		set theRefsNo to count of theRefs
		set theRef to first item of theRefs
		
		-- Error messages	
		if theRefsNo is greater than 1 then error "Select only one item"
		if theRefs is {} then error "Nothing selected in Bookends"
		
		
		-- Get properties of selected reference
		set theID to id of theRef
		set theCitation to user1 of theRef -- user1 is the Bookends field where BibTex citation is stored
		set theAbstract to abstract of theRef
		set theAuthors to author names of theRef
		set theAuthorDate to format theRef using "Author Date.fmt"
		set theTitle to title of theRef
		set theJournal to journal of theRef
		set theFormattedReference to format theRef using "APA 6th Edition Markdown.fmt" --change to reflect the formatted reference required
		set theNotes to the notes of theRef
		set theLink to user4 of theRef
		
		
		--Error message if no DEVONthink link exists
		if theLink is {} then error "No DEVONthink link made in Bookends"
		
		
	end tell
end tell

--Format the annotations summary to Markdown
set theUUID to replaceText("x-devonthink-item://", "", theLink)
set theNotes to replaceText("#", "####", theNotes)
set theNotes to replaceText("@", "Page ", theNotes)
set theReference to replaceText("
", "", theFormattedReference)

set theAnnotations to "# Annotations: " & theAuthorDate & "

**Title:** " & theTitle & "

**Authors:** " & theAuthors & "

**Citation:** [" & theCitation & "](bookends://sonnysoftware.com/" & theID & ") 

**Reference:** " & theFormattedReference & "

**Abstract:** " & theAbstract & "

## Annotations


" & theNotes

-- Create Annotations file in DevonThink
tell application "DEVONthink 3"
	set theRecord to get record with uuid theUUID
	set theGroup to create location "/Annotations" in the current database
	set theMDRecord to create record with {name:theAuthorDate & " (Annotations)", type:txt, URL:theLink, content:theAnnotations} in theGroup
	set name of theMDRecord to theAuthorDate & " (Annotations).md"
	set theMDLink to the reference URL of theMDRecord
	add custom meta data theMDLink for "annotationlink" to theRecord -- change the field name to suit own purposes/custom meta data field
	
end tell

--Replace text subroutine
on replaceText(searchString, replaceString, theText)
	set AppleScript's text item delimiters to searchString
	set theItems to every text item of theText
	set AppleScript's text item delimiters to the replaceString
	set theText to the theItems as string
	return theText
end replaceText
```

An example of what the annotations summary looks like is below.

![Annotations summary]({{ site.url }}/images/annotations_summary.jpg)

## Creating a reading brief

For important pieces of writing I will create a Markdown reading brief. This is stored in a group in DEVONthink called ‘Reading briefs’. The template creates four headings - Summary, Main Points, Critique, Question. The title, authors, citations, reference and abstracts are pulled from the Bookends reference that is linked in the PDF (created by the link script above). The script creates a link in the PDF in a custom meta data field.

```
-- AppleScript to create a Markdown reading brief from the current selection in DEVONthink Pro using Bookends data
-- Created by Kyle Eggleton April 2019

try
	
	
	tell application "DEVONthink 3"
		set theRecord to the content record of think window 1
		-- Requires a linked reference in Bookends  
		set theURL to the URL of theRecord
		if theURL does not contain "bookends" then error
		open location theURL
		-- Get the URL of the selected record in DEVONthink
		set theLink to reference URL of theRecord
	end tell
	
	tell application "Bookends"
		tell front library window
			set theRefs to selected publication items
			set theRef to first item of theRefs
			
			-- Get properties of selected reference
			set theID to id of theRef
			-- Uses BibKey citation stored in user1
			set theCitation to user1 of theRef
			set theAbstract to abstract of theRef
			-- Uses a custom Bookends format ("Author Date.fmt") to pull out the Author and Date 
			set theAuthorDate to format theRef using "Author Date.fmt"
			set theAuthors to author names of theRef
			set theTitle to title of theRef
			-- Uses a custom Bookends format ("APA 6th Edition Markdown.fmt") to make a Markdown formatted reference
			set theFormattedReference to format theRef using "APA 6th Edition Markdown.fmt"
		end tell
	end tell
	
	-- Creates a Markdown record using the above properties
	-- Places link back to original record
	tell application "DEVONthink 3"
		set theGroup to create location "/Reading briefs" in the current database
		set theMDRecord to create record with {name:theAuthorDate & " (Reading Brief)", type:txt, URL:theLink, content:"# Reading Brief: " & theAuthorDate & "
	
**Title:** " & theTitle & "  

**Authors:** " & theAuthors & "

**Citation:** [" & theCitation & "](bookends://sonnysoftware.com/" & theID & ")  

**Reference:** " & theFormattedReference & "  

**Abstract:** " & theAbstract & "  

## Summary  


## Main Points  


## Critique  


## Question  

"} in theGroup
		set name of theMDRecord to theAuthorDate & " (Reading Brief).md"
		-- Creates a link to reading brief from original record. Comment out two following lines if do not want link
		set theMDLink to the reference URL of theMDRecord
		add custom meta data theMDLink for "readingbrieflink" to theRecord -- change the field name to suit own purposes
		
		-- Opens reading brief to edit
		open tab for record theMDRecord
		activate
	end tell
	
	-- Error messaging	
on error errorMessage number errorNumber
	if errorNumber = -2700 then
		set theAlertText to "Error message"
		set theAlertMessage to "The DEVONthink record has no attached Bookends reference"
		display alert theAlertText message theAlertMessage as critical buttons {"OK"}
	else
		display dialog "Error: " & the errorNumber & ": " & the errorMessage buttons {"OK"} default button 1
	end if
end try

```

An example of what the reading brief looks like is below.

![Reading brief]({{ site.url }}/images/reading_brief.jpg)