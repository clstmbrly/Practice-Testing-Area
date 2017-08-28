So You Want Indents and Lists, Huh?
By Joe Burns


..Use these to jump around or read it all!
[Indenting Paragraphs]
[Bullet Lists]
[Square Bullets]
[Number Lists]
[Roman Numerals]
[Letters]
[Start Count After One]
[Put Them Together]
[Definition Lists]
I have received a good many letters asking how I do indented paragraphs and those bullet lists. The little bullets aren't images. They are placed there through HTML Commands. In fact, the entire list format can be created through commands. I'll show you how.
Indenting Paragraph

     I'm surprised that people ask me how I indent paragraphs because I seldom use it. 
     If you'd like to write with indented lines of text, I'd be more than pleased to show you how I do it. I'm sure there's another 
     method for this, but I like the way I do it. I'm comfortable with it.
     My guess is I don't do it a normal, or HTML, way. I simply indent by adding blank spaces. 
     "BUT MY BROWSER IGNORES MY SPACES!!!" you say. 
     Yeah, mine does too. I understand totally. I use this small code to create each of my spaces: &nbsp;
     That thing is an ampersand command that creates a space as if you pushed the space bar. I have a whole tutorial 
     on those commands right here if you like to see more.
     This is what each of these indented lines look like:
     
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;It looks like this. See the five spaces? No, really. That's what I do. Look at the VIEW SOURCE 
     if you don't believe me.
     And that's all I have to say about that. (Joe Gump) There may be another method, but I like this one. So there, 
     (add raspberry sound effect).
     
     Bullet Lists

These lists are nice. Here's why I like them...
They present information in an easy fashion.
The bullets look cool.
They make me happy.
     Sorry about that last one. I just needed another item to make a three-item list. OK, here's how I did it.
<UL>
<LI>They present information in an easy fashion.
<LI>The bullets look cool.
<LI>They make me happy.
</UL>
      Don't be put off by the commands. It's actually only two being used again and again. Here's what's happening.
UL stands for Unordered List. It means bullets will be used rather than numbers. Numbers will be explained later.
LI stands for List Item. It denotes the next thing that will receive a bullet. Please note that no </LI> is required. Neither is a 
Break or Paragraph command. The LI does all that good stuff for you.
/ULends the entire list.
HINT: If you use a center command before this, it doesn't center the entire list, it centers each item messing up the look of the list.
If you would like to move the list closer to the center of the page, simply add more <UL> commands. I have found that three moves the 
list almost to the center. Just remember, if you use three UL commands you need to offer three /UL commands. Like this:
<UL><UL><UL>
<LI> list item
</UL></UL></UL>
I don't like round bullets, I want squares!!!

     Easy now there fellah, you can have your list and squares too. Simply add the command TYPE="square"into your UL command. 
     Like so:
<UL TYPE="square">
<LI>List Item 1
<LI>List Item 2
<LI>List Item 3
</UL>
This is what you get...
List Item 1
List Item 2
List Item 3
Number Lists

     If you would like to create a list that numbers the items rather than just putting a bullet in front, HTML could do that for 
     you too. Yeah, you could just number the things yourself, but that's no fun. It's time consuming too. Dig this:
List Item 1
List Item 2
List Item 3
...and here's how I did it:
<OL>
<LI>List Item 1
<LI>List Item 2
<LI>List Item 3
</OL>
     Notice it's just the same format except I have <OL> where <UL> used to be. Nothing to it. The browser will continue to count 
     up as long as you keep putting <LI> items after the OL. By the way, "OL" stands for Ordered List.
But I Want Roman Numerals!!!

     Arabic isn't good enough for you, huh? Well simply place a TYPE="I"inside the OL command. Notice that is a capital "I" and 
     not the number one. Here's what you get:
List Item 1
List Item 2
List Item 3
...and here's how I did it:
<OL TYPE="I">
<LI>List Item 1
<LI>List Item 2
<LI>List Item 3
</OL>
Roman Numerals?!?! I Want Letters!

     You're just no fun, you know that? OK, regular letters. I can do that. Try this:
List Item 1
List Item 2
List Item 3
...and here's how I did it:
<OL TYPE="A">
<LI>List Item 1
<LI>List Item 2
<LI>List Item 3
</OL>
But I Don't Want Capital Letters!

     *sigh*
     You can turn the letters or the Roman numerals to lower case letters by putting the lower case version of the letter in the 
     OL tag. like so:
<OL TYPE="i">

and
<OL TYPE="a">
     Give it a shot.
Start the Count After One

     Maybe you don't want your count to start at one every time. That's easy to fix. Here's an ordered list that starts at four.
List Item 1
List Item 2
List Item 3
...and here's how I did it:
<OL START="4">
<LI>List Item 1
<LI>List Item 2
<LI>List Item 3
</OL>
Try it yourself.
Can I put these together?

     Yes. Just remember to close each one. You could do something like an OL list and under each LI command for the OL, you could put 
     in a small UL. Like so:
Main Heading
List item 1
List item 2
Secondary Heading
List item 1
List item 2
here's what it looks like:
<OL>
<LI>Main Heading
<UL>
<LI>List item 1
<LI>List item 2
</UL>
<LI>Secondary Heading
<UL>
<LI>List item 1
<LI>List item 2
</UL> 
</OL>
     There is a pattern to putting unordered lists under one another.
The first list gives you the solid bullet
The second gives you the empty bullet. You can see that above.
Each list after that gives you a square bullet.
The Definition List

     You can play with the text all you want inside of all these list commands. Bold, italic, and any other one you want will work. 
     There's one more set of list commands that manipulate the text for you.
     The lists above are all single item lists. Each LI command makes one list item. Now check this out:

Here's What's For Dinner

Salad
Green stuff and dressing
The Meal
Mystery meat and mashed yams
Dessert
A mint
...and here's what it looks like.
<H4>Here's What's For Dinner</H4>
<DL>
<DT>Salad
<DD>Green stuff and dressing
<DT>The Meal
<DD>Mystery meat and mashed yams
<DT>Dessert
<DD>A mint
</DL>
Here's What's Happening
I used an H4 command to create a heading
<DL> stands for Definition List. It tells the browser that a double tier list is coming up.
<DT> stands for Definition Term. It's the first tier.
<DD> stands for Definition Description. It's indented and appears to modify the definition term.

Use the list commands and present information to your readers in a smoother fashion than writing long drawn out 
paragraphs with a lot of detail. I use these lists all the time. I think there might be a tutorial here that doesn't 
use one, but I don't remember it. Enjoy, and happy listing.
     By the way, if you liked this tutorial try CSS and Lists.

[Indenting Paragraphs]
[Bullet Lists]
[Square Bullets]
[Number Lists]
[Roman Numerals]
[Letters]
[Start Count After One]
[Put Them Together]
[Definition Lists]
