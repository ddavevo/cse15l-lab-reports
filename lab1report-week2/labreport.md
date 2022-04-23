## Lab 1 Report, How to Navigate the ieng6 Remote Server

_written by Dave Vo_ | _4/8/22, SP22 (Week 2)_


Acclimating yourself to the server-client work environment can open up the collaborative coding space, making project development more efficient from a holistic perspective. Here's how to get the basics down.


## Setting Up Visual Studio Code


Although the standard terminal application that comes with your computer can suffice, having a proper Integrated Development Environment (IDE) will come in handy for coding and convenience purposes. 

Simply go to [Visual Studio Code](https://code.visualstudio.com) and follow the simple downloading instructions.

![](1.%20VSCode.png)

After you successfully install VSCode, you should see something like this when you first launch it.

![](1.5.%20VSCode%20Startup.png)


## Remotely Accessing a Server


To login to the remote server, type the following command:

`$ ssh cs15lsp22[###]@ieng6.ucsd.edu`

![](2.%20SSH%20Login.png)

You will then be asked to punch in your password.

For the first time logging in, you will be met with a few authentication formalities. But, for returning accounts, you will be greeted with this wall of text:

![](2.5.%20Successful%20Login.png)


## Executing Commands


For basic server navigation, you can use commands like `$ ls` and `$ cd` to see and move between directories.

![](3.%20Navigation.png)

You can create text files in servers by either using `$ touch [newFile]` to create an empty file or `$ cat > [newFile]` to create a file and input text into it.

![](3.50.%20Creating%20Text%20Files.png)

To log off of the server, use the `$ exit` command.

![](3.51.%20Navigation%20Again.png)


## Moving Files with `scp`


To copy files from your computer to the server, use the following command:

`$ scp [fileName] cs15lsp22[###]@ieng6.ucsd.edu:~/`

![](4.%20SCP.png)

Depending on where you want the copy to go, you can customize the directory that is stated after the `:` in the command.


## SSH Keys


We can bypass inputting our password by setting up an SSH Key in the server, which elimates the need to tediously retype our password whenever we want to use `ssh` or `scp`. 

### 1.
First step is to make sure you are in the correct directory. In this case, we want to be in our computer's directory, not the Desktop or other folders.

To do this, first run this command:

`$ ssh-keygen`

You will then see these phrases:

```
Enter file in which to save the key (/Users/[computerName]/.ssh/id_rsa):
```

```
Enter passphrase (empty for no passphrase):
```

Feel free to just press Enter/Return for both of these queries, no input is required in order for this to work.

![](5.%20SSH%20Key%20Generation.png)

Pay attention to this line of text, this is the address you need to pass the SSH key onto the server. 

```
Your public key has been saved in /Users/[computerName]/.ssh/id_rsa.pub
```

**NOTE:** It is important you do not mix up your **public key** _(indicated by .pub)_ with your **private key** _(no indication)_. 

The public key is the one you will be copying to the server.

### 2.

To copy the **public key** onto the server, run the following command:

`$ scp /Users/[computerName]/.ssh/id_rsa.pub cs15lsp22[###]@ieng6.ucsd.edu:~/.ssh/authorized_keys`

Once more, you will asked for your password to confirm the command, but once you see the key file **id_rsa.pub** displayed on the terminal, you're good to go for a password-free experience!

![](5.5.%20SCP%20Key.png)


## Optimizing Remote Running


It is possible to run server commands from the client. To do this, you use the `$ ssh cs15lsp22[###]@ieng6.ucsd.edu` command, like how you would login normally. Except, you would insert commands to the right in parentheses, separating commands using semicolons.

![](6.%20Remote-Server%20Commands.png)
