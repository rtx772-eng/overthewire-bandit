# Bandit Level [0]

## Date : 28 May 2026

## Goal : The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

## Commands Used : 

bandit0@bandit:~$ pwd
/home/bandit0

bandit0@bandit:~$ ls -lh
total 4.0K
-rw-r----- 1 bandit1 bandit0 438 Apr  3 15:17 readme

bandit0@bandit:~$ cat readme
Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
If you are following a course, workshop, walkthrough or other educational activity,
please inform the instructor about the rules as well and encourage them to
contribute to the OverTheWire community so we can keep these games free!

The password you are looking for is: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If


## Solution :

. used pwd to know where are located in the terminal 
. used ls -lh to see the content of the current dir in human shape lists
. noticed a readme file 
. read it with cat + file name 

## Password : ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If


## What I Learned :

- keep going 
- starting keep you ahead of most people 



