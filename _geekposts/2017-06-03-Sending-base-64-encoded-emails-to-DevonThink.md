---
layout: post
title: Sending base 64 encoded emails to DevonThink
---
DevonThink-to-go has the ability to create a new document through Base64 encoding. While this is useful for images I thought that it would also be useful for archiving properly formatted emails from Airmail. While I have previously [documented](http://eggleton.co.nz/geekposts/2017-05-10-Airmail-iOS-to-DevonThink/) how to archive an HTML formatted email into DTTG there is one major fault with this. and that is that the email that is created appears as an HTML file in DTTG (rather than an email). In order to get DTTG to recognise that a file is in fact an email there needs to be a [UTI](https://en.m.wikipedia.org/wiki/Uniform_Type_Identifier) associated with the file. For emails the UTI is com.apple.mail.email. Another requirement is header information i.e. 

	From:  
	To:  
	Subject:  
	Date:  
	MIME-Version: 1.0  
	Content-Type: text/html; charset="iso-8859-1"  

The base 64 URL scheme for DTTG allows us to create a document in DTTG with header information, HTML formatted body and a UTI that enables DTTG to recognise the document as an email. There are two steps to this process. The first is to create a custom action in AirMail that looks like this:

![Custom action in Airmail](/images/airmail-dttg.jpg)

The second step is to create a workflow in Workflow.app that separates out the variables, adds header information and the HTML body of the email derived from the variables, then base 64 encodes the resulting text and finally uses that data to trigger the following URL:

~~~
x-devonthink://x-callback-url/createdocument?title=[url encoded title]&source=[Base 64 encoded text]&uti=com.apple.mail.email&location=[url encoded link]&x-success=airmail://  
~~~

The complete workflow is available [here](https://workflow.is/workflows/3ba88ea89d1c409683fe1e318ada145d)