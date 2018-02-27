---
layout: post
title: Templates in DevonThink
---
The Mac version of DevonThink has the fantastic ability to create a series of templates. I have made a number of these to deal with meeting minutes and work journal entries. What is lacking in the iOS version of DevonThink is something similar. However, what I have found is that templates can be created in [Drafts](https://agiletortoise.com/drafts/) and then sent to DevonThink through a Drafts action. 

In Drafts I have created a series of keyboard scripts (visible on the top of the Drafts keyboard), which when triggered will create a template. For example I have templates for meeting minutes, supervision minutes and a work journal for my projects.

![Keyboard scripts]({{ site url }}/images/draft_keyboard.jpeg)

The Javascript that creates the template is rather simple and obviously can be modified for your needs. My script for creating meeting minutes is below. One important item is the link to the CSS that styles the resulting Markdown document. I store my CSS in a nested group, '/Resources/CSS', in my Database. The other piece of Javascript is the insertion of todays date in the meeting minutes.

```
var now = new Date();
var dd = now.getDate();
var mm = now.getMonth()+1;
var yyyy = now.getFullYear();

if(dd<10) {dd='1'+dd}
if(mm<10) {mm='0'+mm}

var minutes = "Meeting minutes " + yyyy + "-" + mm + "-" + dd + "\n<link href=\"/Resources/CSS/Monokai.css\" rel=\"stylesheet\">\n# Meeting minutes " + yyyy + "-" + mm + "-" + dd + "\n\n**Attendees:**\n\n**Location:**\n\n## Minutes\n";
setSelectedText(minutes);
```

Once I have written up my minutes I then need to send them to DevonThink. I have created a Drafts action that prompts me for the Inbox to send the document to. This then triggers a Javascript, which then feeds into a URL scheme. 

![Draft action to DevonThink]({{ site url }}/images/draft_action.png)

The prompt has a key called 'location' and three buttons corresponding to my Inboxes. The Javascript picks up the output from the prompt:

```
var output = draft.getTag("location_button");
if (output == "Research") { 
    inbox = "48479662-0CC8-436F-A79F-F98D720E80DB";
    } 
    else if (output == "Projects") {
    inbox = "228CE4A0-2BF0-429E-AF75-A03EE9B5EFD9";
    }
	else {
	inbox = "C3A7913B-5335-43EA-880A-19842E701750";
	}
draft.defineTag("inbox", inbox);
```

A tag is created from the 'location' input and then is fed to the following URL scheme in the next step of the action:

```
x-devonthink://createmarkdown?title=[[title]]&destination=[[inbox]]&text=[[body]]
```

This then creates a formatted Markdown document in DevonThink.

![Formatted minutes in DevonThink]( {{ site url }}/images/devonthink_minutes.png)