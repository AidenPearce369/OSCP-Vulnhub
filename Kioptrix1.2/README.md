#KIOPTRIX 1.2 WRITEUP

Download and extract the RAR file

Open the Box in VM

Make sure that you are connected in Bridged Mode

![I](pics/1.png)

-----

Lets use ARP table to find the IP address

![I](pics/2.png)

Lets add the IP with the hostname in /etc/hosts

hostname=kioptrix3.com (Given in description)

![I](pics/3.png)

-----

Lets scan the IP with nmap

![I](pics/5.png)

Port22 cannot be used without proper credentials

Port80 displays a webpage

![I](pics/4.png)

-----

Lets run gobuster to find directories on Port80

![I](pics/7.png)

Enumerated the directories,

/gallery seems interesting

-----

Lets use searchsploit with gathered service versions

![I](pics/8.png)

![I](pics/9.png)

We didn't get any exploits

-----

Lets test the login page,

![I](pics/10.png)

So,it runs on LotusCMS

![I](pics/11.png)

There is an exploit for LotusCMS but it doesn't work for this case

-----

Tried SQLi

But it failed :-(

![I](pics/12.png)

-----

Lets move on to /gallery

Here there are many photos

![I](pics/13.png)

By viewing the recent photos,

![I](pics/15.png)

We can sort the 3 photos based on various category

![I](pics/16.png)

When i pass id=1 as id=1' to check SQLi

It breaks, resulting an SQL error in the page

![I](pics/17.png)

So,we need to run SQLmap

sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1&sort=filename#photos" --dbms=MySQL --dbs

It lists the DB in the SQL 

Now,we scan for 'gallery' to list the tables in it

sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1&sort=filename#photos" --dbms=MySQL -D gallery --tables

![I](pics/18.png)

Lets see tha values in the 'dev_accounts' from 'gallery'

sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1&sort=filename#photos" --dbms=MySQL --D gallery -T dev_accounts --dump

![I](pics/19.png)

We get the hash of user's passwords

![I](pics/20.png)

If you want to crack it automatically with SQLmap pass it with dictionary attack

I use hashcat to crack it

![I](pics/21.png)

![I](pics/22.png)

We got the passwords for the usernames

-----

I tried logging in the LotusCMS login panel

But it failed

![I](pics/23.png)

So it should be credentials for SSH

----

For user dreg,

![I](pics/24.png)

Scan for privileges with sudo -l

![I](pics/25.png)

Even SUID binaries didn't give luck

![I](pics/26.png)

-----

For user loneferret,

![I](pics/27.png)

It seems that we can run /usr/local/bin/ht with sudo

"sudo ht"

Tested that we can run /bin/sh and /bin/bash with sudo..failed :-(

Ran "sudo ht"..failed

Set the ENV value of TERM to "xterm"

![I](pics/28.png)

![I](pics/29.png)

-----

Now run "sudo ht"

It opens HT editor with #root privileges,

![I](pics/30.png)

So we can edit any file using #root privileges

Lets go for "/etc/sudoers"

![I](pics/31.png)

Add /bin/sh or whatever program you want to launch with sudo by loneferret

Save the file and exit

(HT editor commands reffered from net Alt+F3 to open file)

-----

Then in terminal launch your desired shell

![I](pics/32.png)

GOT #ROOT SHELL

























