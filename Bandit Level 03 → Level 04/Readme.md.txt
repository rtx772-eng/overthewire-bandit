Bandit Level [3]

## Date : 31 May 2026

## Goal :  The password for the next level is stored in a hidden file in the inhere directory.

## Commands Used : 

 bandit3@bandit:~$ find inhere
inhere
inhere/...Hiding-From-You
bandit3@bandit:~$ cat inhere
cat: inhere: Is a directory
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 Apr  3 15:17 .
drwxr-xr-x 3 root    root    4096 Apr  3 15:17 ..
-rw-r----- 1 bandit4 bandit3   33 Apr  3 15:17 ...Hiding-From-You
bandit3@bandit:~/inhere$ cat ...Hiding-From-You 
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
bandit3@bandit:~/inhere$ 

## Solution :

- first i did want to find the dir with name with < find + dir name > command , so i did find it and notice the hiding from you file so it probably the file with psswd .

- i did not in the first tme notice that inhere was a dir from the level info but it had seems to me from the cat command that does not want to work then i did realize it 

- so i did change the dir to the inhere dir and they was saying a hidden file so i did go with < ls -la > to see all the hidden files in the inher dir 

- so the file was completly visible and i did see the file content with the command < cat ... Hiding-From-You and voila the password was in there 

## Screenshot : 

look in the screenshot folder of this level 

## Password :

2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

## What I Learned :

- read slowly and read all the info befor starting 

- do it in your way even if the path is longer to get the same reward and accomplish the level is better than copying and giving up 

