* seeing permission of file

```bash

# see the permission
ls -l

===================
total 0
# first column is permission
-rw-r--r-- 1 root root 0 Jul 10 14:24 file10
-rwxr-xr-x 1 root root 0 Jul 10 14:24 file9
===================

echo echo hello > file3.sh

# permission denie this is happen when the file is not have dependent permission to run the file
./file3.sh

===================
bash: ./file3.sh: Permission denied
===================

# take it write permission
chmod 744 file3.sh

./file3.sh

===================
hello
===================

# permission for user
chmod u
# permission for group
chmod g
# permission for other
chmod o

# take it permission for excuting the script
chmod u+x file3.sh

ls -l

===================
-rwxr--r-- 1 root root 11 Jul 10 15:42 file3.sh
===================

./file3.sh

===================
hello
===================

# remove permission
chmod u-x file3.sh

# some trick
# it is means that any script that is ( .sh ) it will have excute and write permission for group and other but not have read for group and other
chmod og+x+w-r *.sh

```

---------------------------------------

* multiple command 

```bash

mkdir test; cd test; echo hello

===================
hello
===================

# if you type this command again this will not be excute this command that potentioal to be true if one command not true other command will be run
mkdir test; cd test ; echo hello

===================
mkdir: cannot create directory 'test': File exists
hello
===================

# with and operator of one command not true other command not be run this is true for first but for second and third 
mkdir test && cd test && echo hello

===================
mkdir: cannot create directory 'test': File exists
===================

# in or operator if one command fail to run other command if true will be run 
mkdir test || cd test || echo hello

===================
mkdir: cannot create directory 'test': File exists
===================

```

-------------------------------

* output something to the prompt


```bash
# use up and down arrow to go up and down
# use space to go to the next page
# by default it will output first 10 line in the top
ls /bin | head

===================
[
addpart
apt
apt-cache
apt-cdrom
apt-config
apt-get
apt-key
apt-mark
arch
===================

```

-----------------------------

* typing in multiple line in command line 

```bash

mkdir hello;\
cd hello;\
echo done;

```

------------------------------

* About linux folder in root directory

```bash

[/bin]     contain the binary file means binary of the program or it have the program

[/boot]    contain all the file that is specific for boot time when the machine is booting

[/dev]     contain developer file

[/etc]     short for editing text configuration contain all file for that

[/home]    contain the file and directory for home directory

[/root]    contain the root file and directory

[/lib]     containe all library 

[/var]     contain the variable that se by the machine

[/proc]    contain all file that specific for process

```
-------------------------------

* some basic command

```bash

whoami

# where i am
pwd

cd

# list
ls 

# long list
ls -l

# go back
cd ..

# go back more
cd ../.../......

# go forward more
cd Documents/test/.......

# go to the home directory
cd ~

# go to root directory
cd /

touch file1.txt file2.txt file3.txt file4.txt .....

# order in vertical
ls -1

# move
mv hello.txt /etc

# copy
cp hello.txt /etc

# remove file
rm hello.txt

# remove directory
rm -r hello

# cat content the file
cat file-name.extension

# see the content the file but with more option to see the page seprate
more file-name.extension

# best than more it has this feature that more not have
less file-name.extension

# using head for show the file content it is like cat but with number option to see the line
head -n 5 file-name.extension

# it is like head but do that in down
tail -n 5 file-name.extension

# output the content file1.txt to the file2.txt with override
cat file1.txt > file2.txt

# output the content file1.txt to the file2.txt without override
cat file1.txt >> file2.txt

# output content ( file1.txt file2.txt ) to the terminal 
cat file1.txt file2.txt

# output the content ( file1.txt file2.txt ) to the file ( combined.txt ) with override
cat file1.txt file2.txt > combined.txt

# add line hello to file hello.txt with override
echo hello > hello.txt

# output this command to the (files.txt)
ls -l /etc > files.txt

# this also like this in the above
files.txt < ls -l /etc

# find word hello in file3.sh if exist
grep hello file3.sh

===================
echo hello # this hello should color be red
===================

# find word hello in file3.sh if exist without case sensitive
grep -i Hello file3.sh

# find root in directory /etc/password
grep -i root /etc/passwd

# find word hello in every file without case sensitive
grep -i hello file*

# find hello in directory (.)
grep -i hello .

===================
grep: .: Is a directory
===================

# find word hello in directory . but every directory that is recursivly of it
# this is basicly in every where in this directory you are in this will search every where
grep -i -r hello .
grep -ir hello .

===================
./.bash_history:echo echo hello >> file3.sh
./.bash_history:mkdir test; cd test ; echo hello
./.bash_history:mkdir test; cd test ; echo hello
./.bash_history:mkdir test && cd test && echo hello
./.bash_history:mkdir test || cd test || echo hello
./Documents/file3.sh:echo hello
===================

```

-------------------------------

* rename

```bash

mkdir test

ls 

===================
test
===================

mv test test1

ls 

===================
test1
===================

mv hello.txt hello_new.txt

```

-------------------------------------

* adding user

```bash

useradd -m john

cat /etc/passwd

===================
.
.
.
# now this is add it in the buttom
alex:x:1001:1001::/home/alex:/bin/sh
===================

# chaning the shell for this user from sh (shell) to bash
usermod -s /bin/bash alex

cat /etc/passwd

===================
.
.
.
# now this is add it in the buttom
alex:x:1001:1001::/home/alex:/bin/bash
===================

# watching the password for that user
cat /etc/shadow

===================
.
.
.
# password is encrypted
alex:!:19183:0:99999:7:::
===================

# now let's run the user
# note: this is possible if you are running the root in other terminal if you do that you get this error
docker exec -it -u alex 1c6326fc5ba8 bash

===================
Error response from daemon: Container 1c6326fc5ba84a0c1cc10d62e8d4341f735c119186a6b4e3e7cac22399c8ed34 is not running
===================

# now let's run root in other terminal and alex user in other terminal

docker start -i 1c6326fc5ba8

===================
root@1c6326fc5ba8:/# 
===================

docker exec -it -u alex 1c6326fc5ba8 bash

===================
# $ means you are not the root user you are regular user
alex@1c6326fc5ba8:/$ 
===================





alex@1c6326fc5ba8:/$ cat /etc/shadow

===================
cat: /etc/shadow: Permission denied
===================





alex@1c6326fc5ba8:~$ pwd

===================
/home/alex
===================





root@1c6326fc5ba8:~# pwd

===================
/root
===================




# add user is different from useradd 
# adduser is adding password and some other information that exclusive for that user
# useradd is other os and different part from orginal os different os not different os but os without root user it is exclusive for regular user
adduser andra





root@1c6326fc5ba8:~# adduser andra

===================
Adding user `andra' ...
Adding new group `andra' (1002) ...
Adding new user `andra' (1002) with group `andra' ...
Creating home directory `/home/andra' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for andra
Enter the new value, or press ENTER for the default
	Full Name []: alex andra
	Room Number []: 21
	Work Phone []: 983929823
	Home Phone []: 39829823
	Other []: 329849823
Is the information correct? [Y/n] y
===================

```

------------------------------

* work with group

```bash

#  adding group
groupadd developers

# see the group folder
cat /etc/group

===================
.
.
.
john:x:1000:
alex:x:1001:
andra:x:1002
===================

usermod -G developers alex

grep john /etc/passwd              or            cat /etc/passwd | grep alex

===================
alex:x:1001:1001::/home/alex:/bin/bash
andra:x:1002:1002:alex andra,21,983929823,39829823,329849823:/home/andra:/bin/bash
===================



groups alex

===================
alex : alex developers
===================

cat /etc/group

===================
developers:x:1003:alex
===================

```