# Bandit Level [4]

## Date : 31 May 2026

## Goal : The password for the next level is stored in the only human-readable file in the inhere directory.

## Commands Used : 

```bash
bandit4@bandit:~$ 
bandit4@bandit:~$ ls -lh
total 4.0K
drwxr-xr-x 2 root root 4.0K Apr  3 15:17 inhere
bandit4@bandit:~$ cd  inhere
bandit4@bandit:~/inhere$ cat
^C
bandit4@bandit:~/inhere$ ls -l
total 40
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file00
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file01
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file02
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file03
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file04
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file05
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file06
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file07
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file08
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file09
bandit4@bandit:~/inhere$ ls -lh
total 40K
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file00
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file01
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file02
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file03
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file04
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file05
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file06
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file07
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file08
-rw-r----- 1 bandit5 bandit4 33 Apr  3 15:17 -file09
bandit4@bandit:~/inhere$ cat inhere
cat: inhere: No such file or directory
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ cat ./-file00
d% h U  N?CN qy ▒      B  g4V bandit4@bandit:~/inhere$ cat ./-file01
i ,  %? mҶ Q  3 Z    A ) bandit4@bandit:~/inhere$ cat ./-file02
/  ^    ,  G V     J 
 PTE bandit4@bandit:~/inhere$ cat ./-file03
  k    ;;Nϱ  m R]Y     Ӭ N Imbandit4@bandit:~/inhere$ cat ./-file04
X   - t i $ ʥ  Ev   v qi 1 HE)bandit4@bandit:~/inhere$ cat ./-file05
>   7   vl  Q nk  , 0K  V  XHbandit4@bandit:~/inhere$ cat ./-file06
 c  W$ g     l( 2 Y
 ] L  ]vr bandit4@bandit:~/inhere$ cat ./-file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
bandit4@bandit:~/inhere$ cat ./-file08
KU      g   
  jD\ 9 hx !   o bandit4@bandit:~/inhere$ cat ./-file09
8' bandit4@bandit:~/inhere$ cat ./-file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
bandit4@bandit:~/inhere$ 


## Solution :

- fist i did list the file in human way by < ls -lh > 
- then i noticed the inhere dir just lonely so i did change dir to the inher dir by < cd inhere > 
- so i listed one more time the files by < ls > and find 9 file with numeration from 0 to 9 
- finally i did read every one of them with cat ./-file + number with each file and i did figure out that the file07 is the only human readable file with the password of the next level 

## Screenshot : 

look at the screenshot folder of this level 

## Password :4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw



## What I Learned :

- trying is the key 
- testing 9 easy file with easy command same worthless but in fact teach you the most valuable thing and biggest lesson in this path named patient .
- lazyness is the opposite of hacking 