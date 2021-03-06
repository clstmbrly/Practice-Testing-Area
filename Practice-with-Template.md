This page serves as the template for creating your own guidance document that you can add to one of the FICAM Playbooks. For an example of a completed guidance document, visit the 'How do I PIV enable my network logon?' page here [note that this link goes to a 404 Error page]. To create your own page, copy this file, save it with a new name (such as mypage.md instead of template.md, as you should not save your information within the template.md file), and change the title and permalink in the top section of this file. The title should be what you want to appear at the top of the page on the site, and the permalink text should match the name of the file, but in the format /mypage/ instead of mypage.md. Throughout this file, you will see text that explains how to correctly use this template and comments explaining the nature of the code that you will encounter.

3. We can put fenced code blocks inside nested bullets, too.
   1. Like this:

        ```c
        printf("Hello, World!");
        ```

   2. The key is to indent your fenced block by **(4 * bullet_indent_level)** spaces.
   3. Also need to put a separating newline above and below the fenced block.


Fenced code blocks inside ordered and unordered lists

This is a numbered list.

I'm going to include a fenced code block as part of this bullet:

Code
More Code
We can put fenced code blocks inside nested bullets, too.

Like this:

printf("Hello, World!");
The key is to indent your fenced block by (4 * bullet_indent_level) spaces.

Also need to put a separating newline above and below the fenced block.




We can put fenced code blocks inside nested bullets, too.

Like this:

printf("Hello, World!");
The key is to indent your fenced block by (4 * bullet_indent_level) spaces.

Also need to put a separating newline above and below the fenced block.

<script> $(function() { $( "#accordion" ).accordion({ heightStyle: "content", collapsible: "true", active: "false" }); }); </script>
Difficulty: Advanced
Overview

This text should be populated with a brief overview of what content the guidance document contains and may include information on the intended audience, the intended outcome of the guide, and any other information that would help the user to understand the guide.

## Assumptions
- This is a bullet item.
- Another item.
- Another item.

  * This text will be a bullet explaining a precondition or assumption before the user begins to follow the steps outlined in this document
The asterisk symbol denotes a bulleted item, and an indented asterisk denotes a second level bullet
This is a second level bullet
Before you get started

This text will provide any reference information that may be needed to complete the steps outlined in the guide.

This text names a link to a reference document [with the hyperlink text within brackets](and the actual URL within parentheses)
Complete the following tasks:

1. Title of Procedure 1
2. Procedure 2
3. Procedure 3
    a. Subprocedure 1
    b. Subprocedure 2

This text will appear as a 'warning flag' on the website, which is a yellow banner. (The ">" symbol and the line directly underneath this body of text create the formatting for this flag.) Warning flags can be used for notifications such as notifying a user that they should skip a certain procedure. {:class="warning"}
This text will appear as a red banner, for an 'alert' message. Alert flags can be used for notifications such as common problems that may occur. {:class="alert"}
This text will appear as a green banner, for an 'informational' message. These flags can be used for notifications such as useful links or helpful tips. {:class="info"}
This is the main body text that explains the purpose of the procedure and any context that you might need before diving into the individual steps. The text within each step should walk the user directly through exactly what they need to do to complete the procedure. Text within double asterisks will appear as bolded. Text within single asterisks will appear as italicized. For more information on formatting in markdown, go here.

Step 1 of the procedure...
Step 2 of the procedure...
This text is for separating a procedure into separate processes, if needed.

Step 1
Step 2
Step 3
2. Title of Procedure 2

Introductory text for the procedure with numbered steps.

Step 1
Step 2
3. Title of Procedure 3

Introductory text for the procedure. The text below is tabbed twice, which is used for creating a code block to show commands that the user will need to execute a process.

    code goes here
References

Any referential links should be added to this section. Links appear in the following format [hyperlink text]({{ site.baseurl }}{{ page.url }})
