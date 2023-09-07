---
layout: post
title:  "Bandit Wargames"
date:   2023-08-09
categories: jekyll update
---

![a key](https://unsplash.com/photos/1bjsASjhfkE)
## Bandit Primer
# SSH and RSA
The bandit wargames make frequent usage of ssh (secure shell) to log into the game server. Secure shell is a way of running commands on a remote server. To initiate an ssh connection, a command is typed in the terminal if you're using macos or linux, or the powershell if you're using a windows operating system.
```bash
ssh [user]@[ip address or hostname]
```
For example, if I want to connect as user bandit0 to the bandit wargames server, I can execute the following command.
```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```
Additional flags can be used such as the `-p` flag to specify a port and the `-i` flag to specify a key file used for passwordless entry.

SSH makes use of asymmetric encryption for purposes of authenticity and confidentiality. What does this mean? Let's take RSA, an asymmetric encryption algorithm as an example. It is called asymmetric because it uses two keys. One key is used to encrypt the contents and the other is used to decrypt it. This is different than symmetric algorithms which use a single key to encrypt and decrypt contents. One key is known as the private key and the other is known as the public key. Private keys are never shared and are kept secret forever. Public keys on the other hand can be shared.

What is the benefit of two keys? Imagine we have two people: Mac and Charlie. Mac wants to send Charlie encrypted information in which only Charlie can decrypt it, in other words he wants confidentiality. Mac can encrypt a message using Charlie's PUBLIC key. Then, Charlie can decrypt it using Charile's PRIVATE key. Other people might know of Charlie's PUBLIC key, but only Charlie can decrypt the message because no one has Charlie's PRIVATE key.

Another usage is Authenticity. What if Charlie is sending a reply to Mac and he wants to ensure that Mac knows that the message is definately coming from Charlie (not an imposter). Charlie can encrypt the message using his own (Charlie) PRIVATE key. Then, Mac can try to decrypt the message using Charlie's PUBLIC key. If it works, he knows that the message came from Charlie. In this situation, other people could theoretically intercept the message and decrypt it with Charlie's public key, in other words, it doesn't make the message confidential, it only ensures that Charlie was the one who sent the message.

Now Mac is sending a reply to Charlie and he wants Authenticity and Confidentiality, how can he achieve this? Mac can encrypt his message using his own (Mac) private key. Then, he can encrypt the message again using Charlie's public key! When Charlie receives the message, he can decrypt it using his own (Charlie) private key and than decrypt it again using Mac's public key. At his point he knows that the message is an authentic Mac message and that no one intercepting the message could have decrypted it!

SSH makes usage of such asymmetric encryption in order to secure traffic from your computer to the remote server. A good way to practice these concepts is to make a ubuntu VM using something like virtualbox. Here are some research topic to google.

concepts:
```
the known_hosts file
ssh fingerprint
ssh with an identity (rsa key) file (-i flag)
preventing password authentication through ssh
```

commands:
```bash
man ssh-keygen
man ssh
man scp
```

files and directories:
```
.ssh/
known_hosts
/etc/ssh/
/etc/sshd_config
/var/log/auth.log
```

## Level 0
Our first task is to SSH (secure shell) into the game server. This can be done using the ssh command with the port specified. After that, we can use cat to print the contents of the readme to the terminal.
```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
cat readme
```

## Level 1
For this level we need to view the contents of a file that beings with a dash. We can reference this file by first specifying the current directory using `./`. We can then cat the file as normal.
```bash
cat ./-
```

## Level 2
This level introduces a file with spaces in the name. We can access this file by escaping the spaces. Keep in mind that pressing tab will autocomplete the name, which is easier than manually typing in the backslashes.
```bash
cat spaces\ in\ this\ filename
```

## Level 3
For Level 3, we need to print the contents of a hidden file. Using certain flags, we can modify the `ls` command to include hidden contents. 
```bash
cd inhere
ls -a
cat .hidden
```

## Level 4
Level 4 asks us to find and print the ASCII file in the directory. We can use the `file` command to determine the file types of the files. The `*` wildcard character can be used to reference all of the files in the directory.
```bash
cd inhere
ls
file ./*
cat ./-file07
```

## Level 5
To clear this level, we have to find a file with certain parameters. Luckily, the `find` command is awesome.
```bash
find . -type f -size 1033c
```

## Level 5 Alt
Here is the solution I used before I learned about the file attribute flags demonstrated above. I used two python modules for this one, Subprocess and RE. Subprocess allows us to run bash commands and regular expressions will help us filter our results. First, we'll run `find` to list the entire contents of the inhere directory. Then, we'll use `split()` to create a list of files. After that, we'll build our lambda function which will run the `stat` command on a particular file. The `stat` command will list attributes regarding the file in question, including the size of the file. Now, we can construct our re string which we'll use to filter the results. The file we are looking for has a size of 1033 bytes. Finally, we can use a list comprehension to filter our results, exit the interpreter using `exit()`, and `cat` the appropriate file. 
```bash
python3
```
```python
import re
import subprocess
l0 = subprocess.run(["find", "."], capture_output=True).stdout.decode("ascii")
l0 = l0.split()
f0 = lambda x: subprocess.run(["stat", x], capture_output=True).stdout.decode("ascii")
res = "Size: 1033"
[x for x in l0 if re.findall(res, f0(x))]
```

## Level 6
For this level we can utilize useful flags from the find command. Searching in root is going to give us a lot of permission errors so we can send them to the oblivion of `/dev/null`.
```bash
find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null
```

## Level 7
Level 7 tells us that the password is next to a certain word in a text file. We can use `grep` to search within the text file for that word.
```bash
grep "millionth" data.txt
```

## Level 8
We can use the power of the `uniq` command to solve this level. The `-c` flag will count the number of occurences of each line and insert it into the printed result. We can then pipe this into grep to find results that appeared only once.
```bash
cat data.txt | sort | uniq -c | grep "1 "
```

## Level 9
For level 9, we can pipe `strings` into a grep.
```bash
strings data.txt | grep "== "
```

## Level 10
The `base64` command finishes this level.
```bash
base64 -d data.txt
```

## Level 11
For this one, we can use the `tr` command to translate the characters by the desired amount. Moving the character a 13 places results in a n. Moving the character m, result in a z, meaning that m is the last character we can rotate before we have to circle back to a. Moving n 13 places results in an a as expected. With this knowledge, we can repeat the same thought process for uppercase letters.
```bash
echo [string] | tr [a-mn-zA-MN-Z] [n-za-mN-ZA-M]
```

## Level 11 Alt
Once again, I'll show the method I used before learning about the `tr` command. We can start the python interpreter by entering `python3` into the terminal. Then, we can create a variable that stores the string we want to decode. Now, let's convert the string into a list of numbers using a list comprehension, the `ord` function will transpose the characters into their numerical representation. Now, let's do some digging to find out what the numerical representations are for the lower and upper ranges of characters we could potentially come across. For example A is 65 whereas Z is 90. Let us now make a function that adds two numbers and circles back to the lower range if the result exceeds the upper range. One way to think about this is adding 1 to the character Z. If we add 1 to Z, we want to get A back, in other words, 90 + 1 should equal 65. After crafting our function, we need to make another function to ensure that special characters and numbers are ignored. After that, let's make a function that utilized both of the previous functions to correctly rotate the numbers. Finally, we can use the `join` function on a list comprehension to get our result.
```bash
python3
```
```python
s1 = "[string contents]"
l1 = [ord(s1[x]) for x in range(len(s1))]
ord('a') #97
ord('z') #122
ord('A') #65
ord('Z') #90
ord(' ') #32
addwithrange = lambda num,by,lower,upper: num+by if num+by < upper+1 else numb+by-upper+lower-1
isspecial = lambda x: True if x < 65 or x > 122 or (x > 90 and x < 97) else False
getnewnum = lambda num,by: num if isspecial(num) else addwithrange(num,by,65,90) if num<91 else addwithrange(num,by,97,122)
"".join([chr(getnewnum(x, 13)) for x in l1])
```

## Level 12
For this level we need to un-hexdump a file and then decompress it a bunch of times. Let's start by making a temp folder to perform our commands.
```bash
cd $(mktemp -d)
mv ~/data.txt data
```
We can use xxd to reverse the hexdump.
```bash
xxd -r data datar
```
Let's use file to determine the type of file. The shell tells us that it gzip compressed. So, let's rename it to use the gz file extension and then decompress it.
```bash
file datar
mv datar data.gz
gunzip data.gz
```
Using file again, we can see that it is bzip2 compressed.
```bash
file data
bzcat data > data2
```
We will have to decompress the file a couple more times after this. Make sure to use `file` to see what type of decompression you should perform after each decompression. At certain points, in addition to `gunzip` and `bzcat`, you will have to use `tar -xf` as well.

## Level 13
Upon logging into bandit13 and using `ls`, we are greeted with a single file. The file is a private key for ssh purposes. Let's copy it back to our personal machine and use it to ssh into level 14. Once we attempt to use the key, we will get an error stating that the key's permissions are too permisive, we can remedy that with `chmod`.
```bash
exit
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
chmod 400 sshkey.private
ssh -p 2220 -i sshkey.private bandit14@bandit.labs.overthewire.org
```

## Level 14
For this level we need to submit the current password to a certain port. We can use netcat for this.
```bash
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

## Level 15
For this level we need to submit the current password to a specific port using SSL. We can use `openssl` for this.
```bash
cat /etc/bandit_pass/bandit15 | openssl s_client -connect localhost:30001 -ign_eof 
```

## Level 16
First, lets find which ports are open. We can use the `nc` command with the `-zv` flags to scan a range of ports. `2>&1` is used to send errors into the standard output and `grep` is used to filter out connections that were refused. Finally, we can use `awk` to print the specific column of information we want: the port number. 
Now that we have the port number saved to a variable, we can view the results and then one by one, test them using the `openssl` command. The correct port will give us a private key which we can use as an identity file (`ssh -i`) when we connect to the next level. After creating the file and copying the key into it, we need to alter the permissions using `chmod`.
```bash
ports=$(nc -zv localhost 31000-32000 2>&1 | grep -v "refused" | awk '{print $5}')
echo $ports
cat /etc/bandit_pass/bandit16 | openssl s_client -connect localhost:31790 -ign_eof
```
```bash
touch [keyfile]
#paste key into keyfile
chmod 400 [keyfile]
```

## Level 17
For this level, we can the `diff` command to see what lines are different among the two files. We could also use vim with the `-d` flag to see a larger representation.
```bash
diff passwords.new passwords.old
```

## Level 18
Level 18 kicks us out as soon as we ssh, but that's okay, we can run commands using ssh.
```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org "cat readme"
```

## Level 19
This level introduces special permissions. If we take a look at the executable file, we can see that the file's group is bandit19 (us) and the user is bandit20. However, the special user bit (SUID) is set for this file because the `ls -l` result is depicting an `s` where an `x` would normally be. This means that when we execute the file, we will be treated as bandit20, not bandit19. We can show this by executing the `whoami` command. With this information, we can print the next password.
```bash
ls -l
./bandit20-do
./bandit20-do whoami
./bandit20-do cat /etc/bandit_pass/bandit20
```

## Level 20
```bash
nc -lp [port] < /etc/bandit_pass/bandit20
```
```bash
./suconnect [port]
```

## Level 21
For level 21, we need to dig into some cron jobs. Once we `cd` to the `/etc/cron.d` directory, we can `cat` the contents of the files. With this information, we can `cd` to the directory of the shell scripts in question: `/usr/bin`. A `ls -l cron*` command will list the right files along with permissions. Perusing the permissions will tell us that we have group read access to a particular script which we can analyze with vim. Analyzing the file will tell us that the script is putting information into a specific file, we can cat that file to get the information we need.
```bash
cd /etc/cron.d
cat cron*
cd /usr/bin
ls -l cron*
vi [file]
cat [path]
```

## Level 22
Based on our experience playing these bandit games, we can assume that the next password is related to bandit23. Using similar detective skills from the previous level, we can analyze the relevant files and end up back in `/usr/bin` to analyze the bandit23 shell script being run. Looking at the script we can see that its using the result of the `whoami` command in several places. Running the `whoami` command in the terminal will return bandit22. In other words, if we run the script as bandit22, it will pull the bandit22 password and input it into a file. However, we want the bandit23 password, not the 22. Looking back at the relevant file in `/etc/cron.d`, we can see that the cron job is being run every minute and that it will run as user bandit23! Let's swing back to the script and see where it's putting the file. Once again, it's using the result of the `whoami` command and the `md5sum` command to create the directory name. So, we can find the directory by running the commands as if we were bandit23!
```bash
echo "I am user bandit23" | md5sum
cat /tmp/[path]
```

## Level 23
This level follows a similar pattern to the previous cron oriented challenges. We should check out the bandit24 cron job in /etc/cron.d to discover the location of the script being run (/usr/bin), and then analyze that script. The script is running and then deleting scripts placed in /etc/spool/bandit24/foo. So theoretically, all we have to do is make a script that will cat the contents of the bandit24 password file and then save it to a file. After we make our script, we should use `chmod` to make it executable and then `cp` the script into the foo directory.
```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 >> /tmp/blarg.txt
```

## Level 24
For this level we need to brute force a 4-digit pin and send it along with a password to a specific port.
```bash
echo "$(cat /etc/bandit_pass/bandit24) 1234" | nc localhost 30002
```
Let's make a script to automate the brute force aspect of it.
```bash
#!/bin/bash
pass=$(cat /etc/bandit_pass/bandit24)
for i in {0000..9999} ; do
echo "$pass $i" >> [textfile]
done
``` 
Then, you can make it executable and execute it.
```bash
chmod 700 [script]
./[script]
```
Now that we have our large text file, we can pipe it into netcat.
```bash
cat [textfile] | nc localhost 30002 > [textfile2]
```
After running this command, I waited for some time. The command appeared to hang so I used control-c to stop it. I then examined the file using word count with the line flag set.
```bash
wc -l [textfile2]
tail [textfile2]
```
The command had created roughly 6200 lines before hanging. After some thinking, I decided to just delete the first 6000 lines of the large text file, thinking that perhaps there was some kind of limit. I did this with vim.
```bash
vi [testfile]
```
Once in vim, I typed `dd` to delete the first line. Then, I typed `6000` and `.` to repeate the `dd` command 6000 more times. I then saved and exited and retried catting the test file into `nc` and this time it worked. I opened the result in vim, hit `G` to go to the bottom of the file, and got the answer.

# Level 25 and 26
There's always something to learn in Linux! Using `ls` in the home directory shows us a single file, it's an identify file for use with `ssh`. However, let's do some digging first. According to the instructions, there's something odd about the given shell for bandit26. We can see everyone's shell by catting the passwd file, let's do that.
```bash
cat /etc/passwd | grep "bandit26"
```
Upon doing this, we can see that instead of the usual `/bin/bash`, a different shell is being referenced. Let's navigate to that directory and cat the contents of that file.
```bash
cat /usr/bin/showtext
```
The script in question is using the `more` command. The `more` command is similar to the `cat` command but is used for longer files. Something I didn't know is that if the amount of text is small enough, the `more` command will behave similarly to `cat` while if the text is quite long, it will enter the interactive mode similar to `less`. Neverless, let's forget about that for a second and try simply using that identify file. We can exit and then use `scp` to copy the identify file to our machine.
```bash
exit
scp -P 2220 bandit25@bandit.labs.overthewire.org:[identifyfile] .
```
Now let's try to log into bandit26.
```bash
ssh -p 2220 -i [identifyfile] bandit26@bandit.labs.overthewire.org
```
Similar to a previous level, we immediately get kicked out. Let's use that `more` knowledge now. Modify your terminal window so that's it's very small and repeat the command, this time it should enter the interactive mode. At this point we can type `v` to enter vim. Another thing I didn't know is that you can modify your shell from vim and even create a shell.
```bash
v #should be in vim now
:set shell=/bin/bash
:shell #should have a shell after this command
ls
./bandit27-do cat /etc/bandit_pass/bandit27
```

## Level 27
For this level we need to clone a git repo. Let's make temporary place to put our stuff first.
```bash
mktemp -d
cd [whatever path it made]
```
Now let's run a git command to clone the repo. Don't forget to specify the port as typed below!
```bash
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
```
After typing in the current level's password when prompted, we can investigate the repo and easily find the next password!

## Level 28
This one looks similar to the last one! Let's start by making our temp directory.
```bash
cd $(mktemp -d)
```
Now let's clone the bandit28 repo (using port 2220).
```bash
git clone [bandit28repo] 
```
Upon investigating the repo, we can see that the readme does not provide the password. Let's look at the log.
```bash
git log
```
The log mentions something about fixing a leak. Let's look at the commit before the leak was fixed.
```bash
git checkout [previous commit id]
```
Now, when we look at the readme, the password is exposed!

## Level 29
More git stuff! Follow the previous instruction and get your git repo then start investigating. This repo has multiple branches. We can look at different branches by using git commands.
```bash
git branch -a #list branches
git switch [branch]
```
If you switch to the dev branch and take a look at the readme, you can find the password!

## Level 30
As usual, let's clone the git repo and start investigating. This time the repo is quite barebones. It seems like git logs and branches won't help us here. Git has many ways of storing and organizing data. One of these methods is using tags. Tags are used to mark important moments in the repo's history; like a new version. Let's see if there are any tags, then we can use `git show` command to see information about the tag.
```bash
git tag
git show [tagname]
```

## Level 31
Clone the repo and look at the readme. This time we get to push stuff! First, let's see what our current brach is.
```bash
git branch
```
We're on the right branch so now, let's make the file.
```bash
touch key.txt
echo May I come in? >> key.txt
```
Before we go further, let's make sure our gitignore file is in order. The gitignore file is used to filter some files from getting into the online repo. The gitignore file should be in the root git directory.
```bash
ls -lah
vi .gitignore
```
The gitignore is filtering out txt files! In vim, we can hit `dd` to delete that line, then `wq` to write and quit. Now let's add and commit the file to the local repo. After commiting, we will be asked to create a commit message.
```bash
git add key.txt
git commit key.txt
```
Now, the file is in our local repo but we have to push it to the remote one. We can do this using the `git push` command. After doing this, we will get our next password!
```bash
git push origin master
```

## Level 32
This level puts us into a strange shell in which we must escape. Everything we type becomes uppercase. In Linux, commands are case sensitive so `LS` for example will not work. This puts us in a pickle. How are we supposed to execute commands like this? The trick to this level is environmental variables. Open a terminal on your personal machine.
```bash
printevn
```
These variables are already uppercase! We can try various variables.
```bash
$path
$shell
```
The variable we want is $0, it stores the name of the shell.
```bash
$0
```
Now we should be free to type commands as we wish, let's try catting the contents of the password file.
```bash
cat /etc/bandit_pass/bandit33
```
