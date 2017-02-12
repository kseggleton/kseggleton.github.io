---
layout: post
title: Dealing with actions from meetings
date: 2017-01-25
---
Taking meeting minutes is a common task in academia. A critical component of good meeting minutes is recording the actions or tasks that arise out of the meeting. While recording actions is important it becomes a waste of time if those actions are not transferred into a task management system. My current task manager of choice is [2Do](http://www.2doapp.com), which has a great URL scheme for iOS automation. By leveraging the power of [Drafts](http://agiletortoise.com/drafts/), [Workflow](https://workflow.is) and 2Do I can convert my meeting minutes into tasks in 2Do. I achieve this by annotating actions that are required as follows:  

> Sally suggested that the paper on habitual ear wax sculpturing should explore whether there is a relationship with tidal moon pulls. ACTION: do literature review on tidal moon pulls

Once the minutes are complete I trigger a Drafts action that sends the text of the minutes to a workflow in Workflow.app. This workflow (pictured below) extracts the text that follows the word ACTION: and sends that text to 2Do via a URL scheme.

![2Do action workflow]({{ site.url }}/images/action.jpeg)