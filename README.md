Nesting vim todo.txt
====================

This is a fork of the excellent
[todo.txt-vim](https://github.com/freitass/todo.txt-vim). I wanted something
that complimented my personal convention of using (a series of) leading project
names for my todos.

TODO
----

### Hiding Information

The following:

    +meeting
    +meeting +discuss
    2016-05-12 +meeting +discuss Come up with project name
    +meeting +schedule
    x 2016-05-13 2016-05-12 +meeting +schedule Call @Bob re: availability
    (A) 2016-05-12 +meeting +schedule Call @Alice re: availiablity
    2016-05-12 +meeting +schedule Place reservation

displays as:

    +meeting
      +discuss
        Come up with project name
      +schedule
        x Call @Bob re: availability
        (A) Call @Alice re: availiablity
        Place reservation

The projects should support folding to hide all nested todos and sub-projects.

A command should exist to toggle the date visibility.

The "true" line should be revealed if you are in insert mode and your cursor is
placed over it.

Consider: `((x )?(?:\d{4}-\d{2}-\d{2} ){0,2}(?:\+\w+ )*)(?=\+)`


### Sorting

Sorting is *not* performed on a line by line basis. Instead contiguous blocks of
todos with *identical* leading projects/contexts are sorted together. This is
easier to understand by way of example. 

The following:

    +meeting +schedule Call @Bob re: availability
    +meeting +schedule Call @Alice re: availiablity
    +meeting +discuss Come up with project name
    +meeting +schedule Place reservation

becomes: 

    +meeting +discuss Come up with project name
    +meeting +schedule Call @Bob re: availability
    +meeting +schedule Call @Alice re: availiablity
    +meeting +schedule Place reservation

Note how even after sorting, "Call @Bob" is still before "Call @Alice". This is
because both todos have the same leading projects/contexts and are contiguous
(next to one another). 


### Label Creation

To identify the relevant leading projects when things are hidden requires that
todos are created that consist *entirely* of the projects (e.g., `+meeting
+schedule`). These should be able to be generated if missing. 

There should also be a command to clean up any label that lacks associated
todos.

In terms of implementation, you'd probably want to sort the document and then
iterate through, inserting just such a label immediately before any line with
todo content (e.g., "Place reservation") where the preceding line does not have
the *exact* same leading projects.


### Todo Creation

You should be able to assign leading projects to a new todo (as well as adding a
date) by indenting them and running specified command.


### Filtering

It should be easy to filter todos based on arbitrary substrings as well as any
projects, contexts (not just leading ones), or priority. 

Implementation: You'll also need to show any labels that the todo uses. This
should be fairly easy to do with lookaheads.


Original Readme
---------------

### (Apr 2015) Important note: mappings were changed!

As it was suggested on issue
[#28](https://github.com/freitass/todo.txt-vim/issues/28) (and as recommended by
vim's documentation), all mappings were changed to use `<localleader>` instead
of `<leader>`. If you don't have `maplocalleader` set on your environment then
yours is probably `\`. For more information on that regard, please take a look
at `:h <Localleader>`.

### (Jan 2016) Note: Overdue date highlight and Python Optional dependency

A new feature was added to highlight dates in overdue tasks as an Error (as
suggested on issue [#44](https://github.com/freitass/todo.txt-vim/issues/44)).
It depends on a Python library, however, and as such will only be able to work
if your version of Vim was compiled with the `+python` option (as most common
versions do).

If your Vim installation does **not** have Python support, this plugin **will
work just fine** but this feature will be disabled.

### Quick install

    git clone git://github.com/freitass/todo.txt-vim.git
    cd todo.txt-vim
    cp -R * ~/.vim


This plugin gives syntax highlighting to [todo.txt](http://todotxt.com/) files.
It also defines a few mappings, to help with editing these files:

Sorting tasks:  
`<localleader>s`   Sort the file  
`<localleader>s+`  Sort the file on +Projects  
`<localleader>s@`  Sort the file on @Contexts  
`<localleader>sd`  Sort the file on dates  
`<localleader>sdd`  Sort the file on due dates  

Edit priority:  
`<localleader>j`   Decrease the priority of the current line  
`<localleader>k`   Increase the priority of the current line  
`<localleader>a`   Add the priority (A) to the current line  
`<localleader>b`   Add the priority (B) to the current line  
`<localleader>c`   Add the priority (C) to the current line  

Date:  
`<localleader>d`   Set current task's creation date to the current date  
`date<tab>`        (Insert mode) Insert the current date  

Mark as done:  
`<localleader>x`   Mark current task as done  
`<localleader>X`   Mark all tasks as done  
`<localleader>D`   Move completed tasks to done.txt  

This plugin detects any text file with the name todo.txt or done.txt with an
optional prefix that ends in a period (e.g. second.todo.txt, example.done.txt).

If you want the help installed, run ":helptags ~/.vim/doc" inside vim after
having copied the files.  Then you will be able to get the commands help with:
`:h todo.txt`.
