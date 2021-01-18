## CS2030S `vim` Guide

This article is adapted from the notes of the [Unix@Home Workshop](https://nus-unix-workshop.github.io/2021-s1) 
held in August 2020.  

After reading this article, students should

- appreciate the usefulness of learning `vim` and using it as the main source code editor.
- appreciate the efficiency and philosophy of using `vim`.
- have experience navigating around a text buffer and manipulating text in `vim`
- be aware of how to learn more about using `vim`.

To edit our code, we need a proper editor.  Remember that ideally we want to keep our hands on the keyboard and keep ourselves "in the zone" with only the terminal, the keyboard, and ourselves, so we will use a terminal-based editor: no windows, no mouse, no arrow keys, no function keys.  

There are only two respectable, widely available text editors in Unix -- `vim` and `emacs`, which one is better has been an ongoing religious war, but for us in SoC, we use `vim`.

### Minimizing Hand Movements

`vim`, like the shell, aims to minimize hand movements.  Frequently used commands are positioned in convenient places on the keyboard.  Let me give you a few examples.

- To exit vim, type ++shift+z+z++.  Notice that this is located at the bottom left corner of your keyboard.  For normal typing, your left hand is supposed to be placed over the keys ++a++ ++s++ ++d++ ++f++, so you just need move slightly your left pinky to ++shift++ and left ring finger to ++z++ and hit them.

- To move the cursor, instead of using the arrow keys, `vim` uses ++h++ to move left, ++l++ to move right, ++j++ to move down, and ++k++ to move up.  For normal typing, you right hand is supposed to be placed on ++j++ ++k++ ++l++ ++semicolon++, so these arrow keys alternatives are located very near to where your right hand should be!

I have a few more things to say about using ++h++ ++j++ ++k++ ++l++ to replace the arrow keys:

- It is not uncommon for applications to re-map other keys for movement.  Many first-person shooting games uses ++w++ ++a++ ++s++ ++d++ for movement, for the same reason as `vim` -- it is close to the resting position of the left hand on the keyboard.

- The use of ++h++ ++j++ ++k++ ++l++ for movement is more ubiquitous than you think.  In the Web-version of Facebook and Reddit, for instance, you can use ++j++ and ++k++ to move up and down across posts.  On this website, you can use ++h++ and ++l++ to go to the previous page and the next page respectively.

### Multi-modal Editor

`vim` is a multi-modal editor.  While for most other editors makes no distinction between reading and editing, `vim` makes an explicit distinction between the two.  `vim` has two basic modes:

- `NORMAL` mode: where you read, navigate and manipulate the text.
- `INSERT` mode: where you insert the text

As a programmer, having a different `NORMAL` modes makes sense since we spend much time reading and navigating around source code.  Thus, allowing the editing commands to optimized.

In the `NORMAL` mode, you can use any of these keys ++i++ ++s++ ++a++ ++o++ (with or without ++shift++) to switch to `INSERT` mode.  To go back to `NORMAL` mode, press ++esc++.  The keys ++i++ ++s++ ++a++ ++o++ have different meanings, which you will learn later.  

Note that most of the time you will be in `NORMAL` mode.  So a habitual `vim` users would insert some text and immediately switch back to normal mode by hitting ++esc++.

### Tell `vim` What You Want To Do; Don't Do It Yourself

In `NORMAL` mode, you can manipulate text in `vim` by issuing commands to `vim`.  These commands are like a programming language.  It is also not unlike the Unix commands, in that each command does a small thing but can be composed together to perform complex text manipulation.

Let me give an example here.  Suppose you have a sentence:

```
Wherever there is light, there is also a shadow.
```

You want to remove `also a` from the sentence.  

What would you do in a typical text editor?  You can use move your hand away from the keyboard, find your mouse, move your mouse cursor to highlight the text, and then hit ++delete++.  Or you could move the cursor (by mouse or by repeatedly hitting the keyboard) to place the cursor after `a`, and then press ++delete++ six times.

In addition to being tedious, this is error-prone.  You might highlight one additional or one less space, or hit ++delete++ one too many times.

What we are used to do is to perform the action of deleting the words ourselves.  For `vim`, we do it differently.  We need to look for the word `also` and delete two words.  This translate to the command ++slash++ ++a++ ++l++ ++s++ ++o++ ++enter++ ++d++ ++2++ ++w++.   

- ++slash++ triggers a search.  This is an almost universal command -- try ++slash++ on Facebook (web) or on this page.
- ++a++ ++l++ ++s++ ++o++ ++enter++ tells `vim` what you want to search.
After enter, your cursor should be placed at the beginning of `also`.
- ++d++ ++2++ ++w++ tell `vim` to "delete two words".

Instead of worrying about the actual actions to perform the deletion, we issue higher-level commands to describe what we want to do.  This is powerful since this is how our brain thinks -- "I need to insert this here, change this word to that, remove two lines, etc"  All these maps into commands in `vim`.  As a result, once you master `vim` basics, you can type as fast as you think[^3]!  

A common pattern for `vim` command consists of three parts: (i) place the cursor; (ii) performance an action; (iii) move to the new placement of the cursor.  In the example above,
++slash++ ++a++ ++l++ ++s++ ++o++ ++enter++ places the cursor, ++d++ is the action (delete), and ++2++ ++w++ is the movement (move the cursor forward by two words).

Another common command that students used is ++g++ ++g++ ++equal++ ++shift+g++.  This command is used to indent the source code in the current file.  ++g++ ++g++ is the command to place the cursor at the top of the file.  ++equal++ is the the action (indent), and ++shift+g++ is the command to place the cursor on the last line of the file.  

### Be A Good Unix Citizen

Not only the basic commands `vim` adhere to the Unix principles of composability, `vim` plays well with Unix shells, which add additional power to the `vim`.  For instance, if you want to have the standard output from a command paste into the file you are editing, you can run:

```
:r! <command>
```
++colon++ triggers the `vim` command line.  ++r++ ask `vim` to read something and paste it into the current cursor location.  At this point, you can pass in, for instance, another file name.  But here, we enter
++exclam++, which tells `vim` to run a shell.  We then pass the `command` to the shell.  Whatever the command writes to the standard output, will be read and inserted into `vim`.

Want to insert today's date?
```
:r! date
```

Want to insert a mini calendar?
```
:r! cal
```

Want to insert the list of all JPG pictures?
```
:r! ls *jpg
```

You can even pass a chunk of text from `vim` to the standard input of another program, and replace it with what is printed to the standard output
by that program.

## Other Reasons To Learn `vim`

Besides enabling you to type as fast as you think with as few hand movements as possible, there are other reasons to use `vim`:

- `vim` is installed by default in almost any Unix environment.  Imagine if you get called to a client-side to debug a Linux server and you need to edit something -- you can rest assure that `vim` is there.

- `vim` is the only source code editor you need to learn and master.  It works for almost any programming language.  If you use IDE, you have to learn IntelliJ for Java, IDLE for Python, Visual Studio C++ for C++, etc.

- `vim` is extensible and programmable.  It has been around for almost 30 years, and tons of plugins have been written.  Whatever feature you need, there is likely a native `vim` command or a `vim` plugin for that.

The only downside to using `vim` is that it is terminal-based (some considers it ugly) and the steep learning curve.  But, in our experience, students will build up their muscle memory and are comfortable with `vim` after 2-3 weeks of usage.

For CS2030S, there is another practical reason to learn and gain familiarity with `vim`.  The practical exams are conducted in a sandboxed environment, which you can only access through `ssh` via a terminal.  You only have a few choices (`emacs`, `nano`, `vim`) and `vim` is the only reasonable choice. 

[^3]: The book _Practical Vim_ by Drew Neil has the subtitle "Edit text at the speed of thought".

## Setting Up Your `vim` Environment

Like many other Unix programs, you can configure your preferences by creating an `rc` (run commands) file in your home directory.  These `rc` files will be read by the corresponding programs and executed line-by-line as if the text is entered into the program through a keyboard.  You can view these `rc` as a script that will be executed automatically whenever a program starts.

For `vim`, the `rc` file is called `.vimrc`.  The `.` in the front of the file name carries a special meaning in Unix.  It means that this file is hidden -- you won't see it when you `ls`.  Hiding the run command files prevent your home directory from being cluttered.  To tell `ls` to show the hidden files, use the `-a` flag
```
$ ls -a
```

We have created a `.vimrc` file, with CS2030S defaults, for your use.  This is the basis which you can built on. 

To copy this file to your home directory on the PE nodes,
```
$ cp ~cs2030s/.vimrc ~
```

The default `.vimrc` expects a backup directory at location `~/.backup`.  This is where a previous copy of your file will be saved as backup everytime you edit a file.  This feature has saved me countless of time.  You can create a directory called `.backup` at your home directory with:

```
$ mkdir ~/.backup
```

# Lessons

Now, follow through the following to get yourself familiarized with `vim`

## Lesson 1: Navigation

Now, you can stay in your home directory or go back to your workshop directory.

Download the following file for practice using `vim` in this session.
```
$ wget https://raw.githubusercontent.com/nus-unix-workshop/2021-s1/master/jfk.txt
```

The file named `jfk.txt` should be downloaded.  Now let's start your first `vim` session.

```
$ vim jfk.txt
```

When you start, you will be in `NORMAL` mode.  For now, just move around the cursor with ++h++ ++j++ ++k++ ++l++.  Get comfortable with using the keys.

Next, try ++parenthesis-left++ and ++parenthesis-right++ to move forward and backward, sentence-by-sentence.

Next, try ++brace-left++ and ++brace-right++ to move forward and backward, paragraph-by-paragraph.

Use ++0++ to jump to the beginning of the line, and ++dollar++ to jump to the end of the line.

Use ++g+g++ to jump to the beginning of the file, and ++shift+g++ (`G`) to jump to the last line of the file.

Now try ++slash++, type in any word (or prefix of a word) and ++enter++.  This should move the cursor to the beginning of the word.  You can use ++n++ and ++shift+n++ to move to the next match and the previous match.

When you are comfortable with moving around, you can ++shift+z+z++ to exit.

Congratulations, you have just completed your first session in `vim`!

## Lesson 2: Manipulating Text

Now, we are going to open up the same file again and try to manipulate the text.  We are going to stay in the `NORMAL` mode still.

```
$ vim jfk.txt
```

### Deletion

Try ++0++ ++d++ ++3++ ++w++ to move the cursor to the beginning of the line and delete three words.

Press ++u++ to undo.  This is another lifesaver that you should remember.

In `vim`, repeating the same command twice usually means applying it to the whole line.  So ++d++ ++d++ deletes the current line.  Try that.

Pairing a command with ++shift++ (or the capital letter version) usually means applying the action until the end of the line.  So ++shift+d++ deletes from the current cursor until the end of the line.

### Copy Pasting

Hit ++p++ to paste back what you just deleted.  Try moving the cursor to somewhere else and paste.

To copy (or yank) the current line, hit ++y++ ++y++.

Remember that all these commands can be composed using the movement-action-movement pattern.  For instance, ++shift+9++ ++y++ ++shift+0++, which corresponds to: move to the beginning of the sentence, yank, and until the end of the sentence, basically copy the current sentence.

As you have seen in the ++d++ ++2++ ++w++ example, you can preceed an action with a number to repeat an action multiple times.

Try ++y++ ++y++ ++9++ ++p++.  You should be able to understand what just happened!

### Deleting a Character

The ++x++ command deletes the current character.

Try this exercise: At the end of the file `jfk.txt`, there are some typos:
```
libertyi. liberty.
```
Change `libertyi. liberty.` to `libtery.` by positioning the cursor on the second `i` and delete it.  Then use ++shift+d++ to delete the extra `liberty.` at the end of the sentence.

### Visual Mode

In addition to the `INSERT` and `NORMAL` modes, `vim` has the third mode, the `VISUAL` mode.  You can enter the `VISUAL` mode by hitting ++v++.  Once in visual mode, you can move your cursor to select the text and perform some actions on it (e.g., ++d++ or ++x++ to delete, ++y++ to yank).

Hitting ++shift+v++ will allow you to select line-by-line.

The `VISUAL` mode allows us to pipe the selected text to another Unix command, and replace it with the result of that command.

Go ahead and try to select a paragraph in `jfk.txt`, and hit ++colon++.  You will see that
```
:'<,'>
```

appears in the last line of the terminal.  At this point, you can type in actions that you want to perform on the selected text.  For instance,
```
:'<,'>w john.txt
```

will write it to a file named `john.txt`.

But, let's try the following:
```
:'<,'>!fmt
```

`!fmt` tells `vim` to invoke the shell and run `fmt`.  `fmt` is another simple small Unix utility that takes in a text (from standard input) and spew out formatted text in the standard output.  You will see that the width of the text has changed to the default of 65.

You can try something that we have seen before.  Reselect the text, and hit
```
:'<,'>!wc
```

The selected text will be replaced with the output from `wc`.

### The `:` command

You have seen examples of `:` commands for writing to a file or piping selected text to an external command.

The `:` command also opens up a large number of actions you can do in `vim`.  Here are a few essential yet simple commands.

- To jump to a line, hit ++colon++ followed by the line number.
- To open another file, hit ++colon++ and then type in `e <filename>`
- To find help on a topic, hit ++colon++ and then type in `help <keyword>`

Other advanced features such as search-and-replace, changing preferences, splitting windows, opening new tabs, are also accessible from the `:` command.

The `:` command prompt supports ++control+p++ and ++control+n++ for navigating back and forth your command history, just like `bash`.  It also supports ++tab++ for auto-completion.

## Lesson 3: Insert mode!

Finally, we are going to try inserting some text.  Remember, to use `INSERT` mode, we always start with a command ++i++ ++a++ ++o++ or ++s++ (may paired with ++shift++) followed by the text that
you want to insert, followed by ++esc++.

Let's try ++i++ (insert).  Place your cursor anywhere, hit ++i++, and start typing, when you are done.  Hit ++esc++.

You just added some text to the file.

Place your cursor anywhere, hit ++a++ (append), and start typing, when you are done.  Hit ++esc++.  ++a++ appends the text to the end of the current line.

Hit ++o++ (open) and start typing, when you are done.  Hit ++esc++.  ++o++ opens up a new line for the your text.

Hit ++s++ (substitute) and start typing, when you are done.  Hit ++esc++.  ++s++ substitute the current character with your text.

Now try it with ++shift++ and see the difference in behavior.

## Other Useful Commands

### Auto-Completion

You can use ++control+p++ or ++control+n++ to auto-complete.  By default, the autocomplete dictionary is based on the text in your current editing buffers.  This is a very useful keystroke saver for long function and variable names.

### Auto-Indent the Whole File

You can ++g++ ++g++ ++equal++ ++shift+g++ in command mode to auto-indent the whole file.  ++g++ ++g++ is the command to go to the beginning of the file.  ++equal++ is the command to indent.  ++shift+g++ is the command to go to the end of the file.  

### Splitting `vim`'s Viewport

- `:sp file.c` splits the `vim` window horizontally
- `:vsp file.c` splits the `vim` window vertically
- ++control+w++++control+w++ moves between the different `vim` viewports

### Search and Replace

```
:%s/oldWord/newWord/gc
```

`:` enters the command mode.  `%` means apply to the whole document, `s` means substitute, `g` means global (otherwise, only the first occurrence of each line is replaced). `c` is optional -- adding it cause `vim` to confirm with you before each replacement

### Jump to `Foo.java`

Place your cursor on the class name, e.g., `Foo`.  Then hit ++g++ ++f++.  

### Change colors

```
:color <scheme>
```

Example, 
```
:color morning
```

To see the list of installed color schemes, type `:colorscheme` ++space++ ++ctrl+d++

### Ruler and Numbers

If you prefer to show the line number you are on and the column number you are on, adding the commands to `~/.vimrc`

```
set ruler
```

will display the line number and the column number on the lower right corner.

You can also add
```
set number
```

to label each line with a line number.

## Learning More

To learn more about `vim`, we suggest that you run `vimtutor` on the command line and follow through the tutorials.  

You can always `:help <keywords>` to search for the built-in help pages within `vim`.

Once you are comfortable, you can soup up your `vim` with various plugins and learn how to use advanced commands (such as recording macros, folding) that are invaluable for programming.

There are also many video tutorials and resources online, in addition to the [introduction to vim](https://mediaweb.ap.panopto.com/Panopto/Pages/Viewer.aspx?id=85be23af-8b65-4d5f-a164-acaa001edc74) by Yong Qi that we have shared earlier, some interesting ones are:

- [Vim: Precision Editing at the Speed of Thought](https://vimeo.com/53144573): A talk by Drew Neil
- [Vim Adventure](https://www.vim-adventures.com): An adventure game for learning `vim`
- [Vim Casts](http://vimcasts.org/episodes/archive/): Videos and articles for teaching `vim`
- [Vim Video Tutorials](http://derekwyatt.org/vim/tutorials/) by Derek Wyatt
- [Vim Awesome](https://vimawesome.com/): Directory of plugins.
