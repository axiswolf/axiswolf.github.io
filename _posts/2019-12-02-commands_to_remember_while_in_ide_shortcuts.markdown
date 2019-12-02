---
layout: post
title:      "Commands to remember while in IDE (shortcuts)"
date:       2019-12-02 12:39:06 +0000
permalink:  commands_to_remember_while_in_ide_shortcuts
---

*My Notes, My Blog Entry, Good Enough, I guess?*

Sorry everyone, I'm not much of a writer (hence why I never pursued in technical writing). It is never too late to start learning. 
Today's post will be about commands in the terminal that is only mentioned a couple of times or in videos. 
They're small little details but super useful when you're coding. 

The following notes can be found in detail in the lesson plan "Advanced Finding Lab".

**Section 1: Creating In Terminal**

mkdir db --> Make a folder called "db"
mkdir db/migrate --> Make a folder within "db" called "migrate"
touch db/migrate/001_create_shows.rb --> Make a ruby file called 001_create_shows within "db/migrate"


**Section 2: Navigating **

ls --> list where we are in terminal
cd .. --> move up a folder 
cd [choose from the list that you've obtained from the command ls] --> moves us to that folder 

** cd can take you all the way back, so use the command *cd ..* instead.


**Section 3: Deleting Files or Directories**

rm [file name] --> delete [file name]
rmdir [directory name] --> delete [directory name]

**Section 4: rspec **

rspec or learn --> this runs your rspec tests to see if everything passes, however sometimes there will be so many tests and you may find it difficult to find.
A solution for that would be to run:
rspec [file name]

* shortcut tip: if you start typing the file name, and press tab, the IDE will automatically fill out the rest of the file name for you.*



