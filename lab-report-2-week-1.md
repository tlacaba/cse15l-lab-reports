# Lab Report 2 - Week 1: Remotely Connecting to a Computer with SSH!

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

![Image](/VSCodeInstaller.png)

## Remotely Connecting
We will need to install [OpenSSH](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui). Once you have that installed, open VSCode, and you will need to use the `ssh` command followed by your username. For this course, the username will be cs15lfa22##@igetc.ucsd.edu, where the two pound symbols are two alphabetic letters, and you will enter your password when prompted. 

I also already had SSH installed, so here I logged into SSH:

![Image](/LoggingIntoSSH.png)

## Using Commands
Now that you're logged in, you can use some commands to explore the directory, such as `pwd`, `ls`, `cd`, etc. Here I used the former two:

![Image](/UsingCommands.png)

Here's what these commands did:

* `pwd`: prints the working directory.
* `ls`: lists files in the working directory.
* `exit`: signs you out of the remotely accessed computer.

## Moving Files with `scp`
In order to move files to your remote computer, you need to use the `scp` command. The structure of `scp` is as follows:  
``` scp filename.java username@iget6.ucsd.edu:~\```  
where "filename" is the file you would like to move, the file extension can be anything you want, "username" is your own username, and after the ":", you can specify the file path in the server you want to save to. This command will prompt you for your password. 

Here, I copied over, compiled, and used WhereAmI.java:

![Image](/MovingFiles.png)

## Setting an SSH Key
Having to always input your password will start to become a hassle, so to circumvent this, we will make use of SSH keys. We first need to obtain a public and private key file with the `ssh-keygen` command on our local clients. Select the default path for the key file to save in, and enter in a passphrase. The default path should be `/Users/YOUR_USER/.ssh/id_rsa`. Since I am on Windows, I needed to perform the additional step of using `ssh-add`.

Make sure you are running a Powershell as an administrator. In this terminal, you will input the following commands:
```
Get-Service ssh-agent | Set-Service -StartupType Automatic

Start-Service ssh-agent

Get-Service ssh-agent

ssh-add $env:YOUR_USER\.ssh\id_rsa
```
From here, you're going to want to sign into the remote computer, make an .ssh directory with `mkdir .ssh`, then sign back out and send the public key file to .ssh/authorized_keys on the remote computer with `scp`. We will keep the private key for our own use.

Here's me logging in without having to fill out my password:

![Image](/LogInWOPW.png)

## Optimizing Remote Running
If you follow an `ssh` command with another command in quotes, you can run a command on the server and exit. Also, you can run multiple commands on the same line of most terminals with semicolons.

Here, I used both techniques, using one ssh command to print the working directory, and both compile and run WhereAmI.java:

![Image](/MultiCommand.png)
