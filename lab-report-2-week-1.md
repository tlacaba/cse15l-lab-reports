# Remotely Connecting to a Computer with SSH!

In this tutorial we will cover:

* Installing VSCode
* Remotely Connecting
* Using Commands
* Moving Files with `scp`
* Setting an SSH Key
* Optimizing Remote Running

---

## Installing VSCode
You can download the installer for VSCode [here](https://code.visualstudio.com/). Simply allow the installer to do all the work for you. I didn't do this because I already had this installed.


## Remotely Connecting
We will need to install [OpenSSH](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui). Once you have that installed, open VSCode, and you will need to use the `ssd` command followed by your username. For this course, the username will be cs15lfa22##@igetc.ucsd.edu, where the two pound symbols are two alphabetic letters, and you will enter your password when prompted. I also already had this installed.


## Using Commands
Now that you're logged in, you can use some commands to explore the directory, such as `pwd`, `ls`, `cd`, etc. Here I will use

## Moving Files with `scp`
In order to move files to your remote computer, you need to use the `scp` command. The structure of `scp` is as follows:  
``` scp filename.java username@iget6.ucsd.edu:~\```  
where "filename" is the file you would like to move, the file extension can be anything you want, and username is your own username. This command will prompt you for your password.


## Setting an SSH Key
To surmise what needs to be done: We need to obtain a public and private key file. Send the public key file to .ssh/authorized_keys on the server, and keep the public key for our use. Go with the default algorithm and file path.


## Optimizing Remote Running
If you follow an `ssh` command with another command in quotes, you can run a command on the server and exit.

Also, you can run multiple commands on the same line of most terminals with semicolons.