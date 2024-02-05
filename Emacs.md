# Emacs


**Why use Emacs for things like cloud computing even in the modern day?**

Such applications are at the mercy of networking, which are orders of magnitude slower than operations on a local file system. Thus, they care a lot about **throughput** and **latency**, and terminal interfaces can deliver because they are simple and fast.


## Implementation


Emacs is built in "layers": it has a C interpreter inside it, and atop it is a Lisp interpreter. The bulk of Emacs is written in Lisp.

```
+------------------+
| Lisp code        |
+------------------+
| Lisp interpreter |
+------------------+
| C interpreter    |
+------------------+
```

The I/O devices like mouse, keyboard, and display communicate with the application through the Lisp code.

Programs typically have their own interpreters embedded in their own executables. For example, Chromium executables come with a JavaScript interpreter included in them.


## Concept of Buffers


* Emacs makes use of **buffers** to be *fast*. Buffers are just a bunch of text that live in RAM.
* Emacs (and editors in general) makes a clear distinction between what's *persistent* and what's not to balance speed and reliability. For work that is rapidly changing like when you're typing a sentence, we have very performant buffers. Only when work is ready to be saved, the application can flush the buffer to the file system in one fell swoop.


### Auto-generated Files


* By convention, file names starting with `.#` indicate a symbolic link to a file that is currently being edited by another program.
* When editing a file `F` in Emacs:
  - Emacs creates a symlink `.#F` that signals other programs that the file is being edited.
  - Emacs creates a file `#F#`, a copy of the unsaved buffer for `F` as part of its auto-save feature, a safety mechanism for in case Emacs or the computer crashes. This file disappears on write.


## Editor Navigation: Moving the Point


**NOTE:** arrow keys (and their Shift and Ctrl variants) work too, but many of the movement keys are actually universal across Unix, which could be useful to know. Also, it's faster to use key binds that are in the main keyboard area instead of having to reach for arrow keys.

The current cursor position is called the **point**.

From **anywhere**:
```
M-g M-g NUM RET     Go to line number NUM
```

At any point, you can use `C-l` (that's a lowercase L) to vertically center the point if possible. Use `C-l` again to bring the current line to the top of the viewport.


## Help System


The help system makes Emacs a "self-documenting" system. `C-h` is the designated **help key**, which prefixes commands related to viewing documentation.

```
C-h b                           list key bindings
C-h f                           describe a function
C-h k KEYSTROKE(S)              list one binding
C-h a <regex> RET               search for a command
M-x apropos RET <query> RET     search for a command
M-x apropos-command RET         like the one above, used to search certain command
```


## Extended Commands


`C-x` is the designated **eXtended command key**, which starts keystroke chords for common commands, like `C-x C-f` to open a file.

`M-x` is the designated **eXtended named command key**, which focuses a command **minibuffer** at the bottom of the terminal, below the mode bar. The minibuffer is itself a buffer supporting your familiar navigation keys, and in it you can write the full name of commands.

After attempting to use the full name for a command that has a `C-x` shortcut, Emacs will tell you what the shortcut is for future use.

There is a special command that allows you to send a numerical argument to a command:

```
C-u NUM KEY
```

For many commands, this simply repeats the actions, but some commands may have slightly different behavior. For example:

```
C-u 2 C-k       delete content of two lines and their newlines
C-k C-k         delete content of a line, and then its newline
```

This behavior is determined by how they're implemented - whether they take that optional numeric argument in the first place and what they do with it.


## Window and Buffer Navigation

Opening another window for the same buffer does not duplicate the buffer; any edits in one buffer will affect the other(s). You can still use this when you want to reference some other part of the buffer while typing in another region.


## Shell within Emacs

**Q:** What happens if you do emacs within the shell, then do M-x shell to start a shell within emacs, and then type emacs within the emacs shell? Does that work, and if so, for how many times?

**A:** It won’t even really work for one iteration because the emacs shell does not emulate many of the terminal features that emacs needs to run smoothly.

## Selection Manipulation


Concept of the "current region (of current buffer)":

* You can save pointers called **marks** at arbitrary positions within a buffer.
* The **current region** is all the characters between the mark and the point.

You can set a mark at the current point with `C-SPC` or `C-@`. An example of selecting a region of text:

```
M-<             go to start of buffer
C-@             set marker
C-s eggert RET  search for "eggert" forward
C-r             search backward
C-M-s           regex search forward
C-M-r           regex seach backward
M-|             pipe buffer into a shell command
```

You can find out where your mark is with `C-x C-x`, which exchanges point and mark (selects the text between them). You can `C-g` to cancel the selection.


### Modes


Emacs is a **modeful** editor. That means the current state of Emacs not only includes the contents of the current files being edited but also what way you intend to be using the editor next. A **mode** is like a method of interacting with the editor.

* **Upside:** more efficient for experts
* **Downside:** confusing/tricky for non-experts

```
M-x MODENAME            switch to mode
```

`C-h m` brings up a buffer that describes what mode you are in. The default "editor" mode is called **Fundamental** mode, and there is also **Text** mode in which navigating among words with certain characters like apostrophes is slightly different.


### Other M-x functions


```
M-x replace-string        replace string systematicl, forward
M-x flush-lines           delete all lines containing certain strings
M-x compile RET make -k hello ./hello.c       which compiles the c++ file using cmake (can be gcc if wanted)
M-x load RET file_name RET                 load the file E.x. file.el
M-SPC                                      cycling the indicated space between 1, 0, original
M-x delete-all-space
M-: (delete-all-space)

M-x: delete-horizontal-space
M-: (delete-horizontal-space)

M-x delete-region
M-: (delete-region START END)

M-x goto-char RET POSITION
M-: (goto-char POSITION)

M-: (insert YOUR_STRING)                    insert
M-x: insert-char                            (or string)

M-: (elt SEQUENCE INDEX)                    get the element at passed index
M-: (message “%s” this-command)             prints the name of current command
M-: (message "%s" last-command)             same as above
M-: (plist-get PLIST PROPERTY)              prints the corresponding value of passed in 
                                            property in the passed in plist
M-: (prefix-numeric-value &optional ARG)
M-: (functionp object)                      checks if passed in object is a function
M-: (funcall function &rest arguments)      this call function with specific arguments.
```