# Week 7 Lab Report: Vim

For this week's lab, we will be comparing the efficacy of Vim versus other workflows.

Vim is a text editor that was introduced to the public in 1991. It was used during the times where the only interface a computer had was its terminal. To get around the editor, you only use your keyboard, to move your cursor around, edit the file, and everything else. I has a multitude of "modes" when editing, and an even more vast offering of commands and shortcuts.

At first, it may seem that Vim is a useless, hopeless, archaic text editor, but despite it's flaws, it is still used to this day. It is one of the few text editors that can be used entirely in terminal, and mastery of Vim and its breadth of commands allow its users to work more efficiently than in any other text editor.

In this lab, we will compare the execution of various tasks, using Vim and VSCode.

## Task: Changing a word and multiple occurances of the word

In the file `DocSearchServer.java` on an remote ssh server, change the name of the `start` parameter of `getFiles`, and all of its uses, to instead be called `base`.

## Performing task in Vim

Upon opening the document in Vim, the following keystrokes will edit the text file as desired.

* Keystrokes: /star\<ENTER>cebase\<ESC>n.n.n.:wq\<ENTER>

Let's dissect what is going on here:

* **The function of '/':** in normal mode (the mode that Vim starts in), typing '/' will allow you to enter a pattern that Vim will search for and find within the file. Pressing \<ENTER> will exit you from the search, moving your cursor to the location shown in the "search mode" (not really a mode). 

* So, the sequence of typing '/', "star", and pressing \<ENTER> will move the cursor to the location of "start". In this case, we typed: "star", which led us to the first location in which the pattern "star" is found in the file. We didn't need to type "start" in it entirety since, the only occurences of "star" are the occurrences of "start". 

![Image](/vimslashsearch.png)

* **The function of 'c' and 'e':** in normal mode, typing 'c' should be followed by a "vim motion". From the current cursor location to the location reached by the motion, all characters will be deleted and put the editor in INSERT mode (the mode in which you can type in the conventional sense). In this case, 'e' goes to the end of the current word (to the next white space, in essence).

* So, the sequence of typing 'c', 'e', "base" will delete the entire word (in this case, the word "start"), and replace it with base. \<ESC> will return you from INSERT mode. 

![Image](/vimReplaceWord.png)

* **The function of 'n' and '.':** following the use of '/' to search for a pattern, 'n' can be used to move your cursor to other occurrences of the last pattern searched. Following a contiguous series of edits, '.' can be used to repeat the same edits for the current location your cursor is at. ![]

* So the series of three 'n' and '.''s will move your cursor to each occurrence of "start" and replace it with base.

![Image](/vimNDot1.png)

![Image](/vimNDot2.png)

![Image](/vimNDot3.png)

![Image](/vimNDot4.png)

* And finally, ':' lets you open a prompt for various commands, one of which is "wq", which *writes* (saves) and *quits* (exits) the file. \<ENTER> will input the command.

## Comparing to VS Code

**The task we are performing:** In the file `DocSearchServer.java` on an remote ssh server, change the name of the `start` parameter of `getFiles`, and all of its uses, to instead be called `base`.

Timing myself performing the task in Vim and verifying that it worked with a bash shell, I took a total of 55 seconds.

Performing the same edits in VS Code, `scp`-ing the file over to the remote computer, and running the bash shell to verify that it worked, took a total of 108 seconds.

## In conclusion

If I had to make the choice between using Vim and VS Code with the appropriate ssh commands, I would say "it depends". What does it depend on? Well, while Vim is very quick, especially after the mastery of Vim, it does not have all the features that more complex editors like VS Code has. For example, VS Code can come with the "Intellisense" feature that basically finshes your sentences for you. VS Code has error warnings, debugging features, and so much more than Vim has alone. 

On the other hand, Vim is very useful for quick edits that need to be made with code bases. The fact that Vim can be used in terminal will make it an easy choice when needing to make quick edits on a code base stored on a remote server.

All in all, learning Vim will prove to be very useful if one is considering a future career in the tech industry.