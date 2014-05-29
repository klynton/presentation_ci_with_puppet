!SLIDE transition=fade

# CI Tooling #

!SLIDE bullets incremental transition=fade

# Git #

* Most widely used & most widely misunderstood VCS in existance
* Doesn't fit nicely with our existing compute paradigms

!SLIDE bullets incremental transition=fade

##In Exmple: Subversion 
* just files and folders; commits are numbered sequentially. 
* even branching and tagging is simple — it’s just like taking a backup of a folder.

!SLIDE bullets incremental transition=fade

# Git is ... different #

* locally cached VCS - everyone has complete local copy
* makes it super fast!
* makes it easy to run async
* ... but there's some crazy lingo you need to grasp

!SLIDE bullets incremental transition=fade

* **blobs** = the fundamental git *data type*
* **tree objects** = kinda like *directories*, but also kinda like sym links: can contain pointers to other blobs & trees
* **commit object** = *pointer to tree object & metadata*
* **tag object** = *pointer to commit object & metadata*
* **references** = *pointer to tag or commit object*

!SLIDE  transition=fade

# Git Hooks #

SLIDE bullets incremental transition=fade

## what are hooks

SLIDE transition=fade

# Quick CI w/ puppet-lint #

SLIDE transition=fade

# Unit/System/Accceptance Testing #

SLIDE bullets incremental transition=fade

## Unit Testing 

SLIDE bullets incremental transition=fade

## System Testing

SLIDE bullets incremental transition=fade

## Acceptance Testing

SLIDE transition=fade

# Full Circle: Travis CI #

SLIDE transition=fade

# RESTful Deploys #

!SLIDE bullets incremental transition=fade

# r10k #

!SLIDE bullets incremental transition=fade

# A Complete Picture #
