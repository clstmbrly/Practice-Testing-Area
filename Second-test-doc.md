
Jump to:

[Appendix A-Test Header](#appendix-a-test-header)

### [Footnotes](https://github.com/markdown-it/markdown-it-footnote)  From Markdown-it location but it doesn't work here.

Footnote 1 link[^first].

Footnote 2 link[^second].

Inline footnote^[Text of inline footnote] definition.

Duplicated footnote reference[^second].

[^first]: Footnote **can have markup**

    and multiple paragraphs.

[^second]: Footnote text.


Trying an Information box using html from LL .md file:

<div class="usa-alert usa-alert-info">
  <div class="usa-alert-body">
    <h3 class="usa-alert-heading">{{include.heading}}</h3>
    <p class="usa-alert-text">{{include.content}}</p>
  </div>
</div>


This page serves as the template for creating your own guidance document that you can add to one of the FICAM Playbooks. For an example of a completed guidance document, visit the 'How do I PIV enable my network logon?' page here [note that this link goes to a 404 Error page]. To create your own page, copy this file, save it with a new name (such as mypage.md instead of template.md, as you should not save your information within the template.md file), and change the title and permalink in the top section of this file. The title should be what you want to appear at the top of the page on the site, and the permalink text should match the name of the file, but in the format /mypage/ instead of mypage.md. Throughout this file, you will see text that explains how to correctly use this template and comments explaining the nature of the code that you will encounter.

<script> $(function() { $( "#accordion" ).accordion({ heightStyle: "content", collapsible: "true", active: "false" }); }); </script>
Difficulty: Advanced Overview

This text should be populated with a brief overview of what content the guidance document contains.  It may include information on the intended audience, the intended outcome of the guide, and any other information that would help the user to understand the guide.

## Appendix A&nbsp;-&nbsp;Test Header

  * This text will be a bullet explaining a precondition or assumption before the user begins to follow the steps outlined in this document The asterisk symbol denotes a bulleted item, and an indented asterisk denotes a second level bullet This is a second level bullet Before you get started

This text will provide any reference information that may be needed to complete the steps outlined in the guide.

This text names a link to a reference document [with the hyperlink text within brackets](and the actual URL within parentheses) Complete the following tasks:

##Title of Procedure 1
This text will appear as a 'warning flag' on the website, which is a yellow banner. (The ">" symbol and the line directly underneath this body of text create the formatting for this flag.) Warning flags can be used for notifications such as notifying a user that they should skip a certain procedure. {:class="warning"} This text will appear as a red banner, for an 'alert' message. Alert flags can be used for notifications such as common problems that may occur. {:class="alert"} This text will appear as a green banner, for an 'informational' message. These flags can be used for notifications such as useful links or helpful tips. {:class="info"} This is the main body text that explains the purpose of the procedure and any context that you might need before diving into the individual steps. The text within each step should walk the user directly through exactly what they need to do to complete the procedure. Text within double asterisks will appear as bolded. Text within single asterisks will appear as italicized. For more information on formatting in markdown, go here.

Step 1 of the procedure... Step 2 of the procedure... This text is for separating a procedure into separate processes, if needed.

Step 1 Step 2 Step 3 2. Title of Procedure 2

Introductory text for the procedure with numbered steps.

Step 1 Step 2 3. Title of Procedure 3

Introductory text for the procedure. The text below is tabbed twice, which is used for creating a code block to show commands that the user will need to execute a process.

code goes here
References

Any referential links should be added to this section. Links appear in the following format [hyperlink text]({{ site.baseurl }}{{ page.url }})
