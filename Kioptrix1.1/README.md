#KIOPTRIX 1.1 WRITEUP

This is a Boot2Root Machine

Download and Extract the RAR file

And launch it in the VM

It shows that it is running on CentOS 4.5

![I](pics/1.png)

-----

Make sure that you are connected to the host in Bridged Mode

![I](pics/2.png)

Lets check the ARP table to find the IP address

![I](pics/3.png)

We got the IP address

Now lets scan the IP using nmap

![I](pics/4.png)

-----

We can't enumerate SSH port without sufficient details,so skip this

Port80 is opened and interesting

![I](pics/5.png)

In Port3306 it runs MySQL

In Port443 We get 400 Bad Request

![I](pics/6.png)

Others are not helpful

-----

Now in Port80,

There is a login page (index.php)

To find more directories, I ran gobuster 

![I](pics/7.png)

No luck  with the found directories :-(

![I](pics/8.png)

![I](pics/9.png)

Back to main page

-----

Now we dont know the credentials for the login page

But we do know the page is for admin-panel

Lets assume our user is "admin"

I bypassed the login page with simple SQLi

uname:admin
psw: ' or 1=1-- 

![I](pics/12.png)

We BYPASSED

To automate it,We can also use SQLmap to get passwords

The DB its running is MySQL

![I](pics/10.png)

![I](pics/11.png)

-----

After bypassing,

![I](pics/13.png)

We can see the input field is similar to command injection

The "index.php" page redirects us to "pingit.php"

![I](pics/14.png)

Where it gives the ping result of given host value in input field

So i passed inputs with

google.com;ls

![I](pics/15.png)

Bingo!we got the files listed in the directory

![I](pics/16.png)

Lets pass id,

![I](pics/17.png)

-----

So to get a shell to work with,lets pass a bash reverse shell one liner

![I](pics/18.png)

Lets open netcat and start listening

![I](pics/19.png)

When we submit the command in input field, the page keeps on loading and we get a shell

![I](pics/20.png)

----

Now our main task is to elevate our privileges to #ROOT user

So first lets check ,is there any SUID binaries or not

![I](pics/21.png)

No luck :-(

Lets see "sudo -l" is available or not

![I](pics/22.png)

No luck :-(


Lets gather the kernel info

![I](pics/23.png)

Lets search if there is any exploit available for this CentOS 4.5 Kernel in Searchsploit

![I](pics/24.png)

Copy this to a location and launch HTTP server using python

Using wget, download the exploit file

![I](pics/25.png)

Compile the exploit to binary

Lets move to /tmp where there will be no restrictions

![I](pics/26.png)

-----

Once the exploit is executed

We get the #ROOT shell

![I](pics/27.png)



