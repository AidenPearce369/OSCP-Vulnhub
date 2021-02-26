#VULNHUB-KIOPTRIX 1 WRITEUP 

This is a beginner level boot2root box

Download and extract the box

Launch it in VM

Make sure that you are connected to the VM in Bridged Mode

-----

This is how the login screen looks

![I](pics/1.png)

Lets scan the IP from ARP table

![I](pics/2.png)

We got our IP

-----

Lets enumerate this IP using nmap

Enabling Service Version Detection , Script Scanning and Aggresive Scan

![I](pics/6.png)

These ports are available

-----

Our pitfalls,

SSH can't be acessed and we don't know its username yet , so can't be used

Port 80 , looks like normal apache server page

![I](pics/3.png)

Ran gobuster on Port 80 with directory-list-2.3-medium.txt nothing came

Port 111 & Port 1024 - No clue

---

Ran port 443 on browser

It showed like this

![I](pics/4.png)

It seems like there should be 'https://' in the url as it is mentioned in the page

After adding 'https://' and reloading it

It loaded like a normal apache server home page

![I](pics/5.png)

But the SSL/TLS lock icon looks different

-----

Use searcshploit on Apache 1.3.20

![I](pics/7.png)

Many results came

To narrow it down ,

Use searchsploit on mod_ssl 2.8.4

![I](pics/8.png)

It shows OpenFuck exploit 

----

But the exploits available in Searchsploit and ExploitDB are outdated and not configured properly

Lets use OpenFuck Git Repo

![I](pics/9.png)

Follow the steps in their repo

![I](pics/10.png)

-----

Running the exploit,

It shows all possible values for every OS supported by it

We know the box runs on RedHat Linux 7.2 with Apache 1.3.20

It show two possible values

Lets try both

![I](pics/11.png)

So this first value fails, lets try the second one

![I](pics/12.png)

We got the $shell

And we are #ROOT in it

![I](pics/13.png)


