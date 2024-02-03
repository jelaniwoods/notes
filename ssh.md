# SSH

Secure Shell

Used to access a computer securely over an insecure network. 

This prevents packets from being exposed and understood from other computers connected to the same network (unless it's the computer you're ssh-ing to).

### How it work?

- When you `ssh` into pc, you open a TCP connection\* (unencrypted) between the source and destination pc.
	- Channel(s) are created to send the data (and enable multipex??? 😕)
- SSH breaks data down into packets
- payload (important data in packet) is encrypted
- server receives packets and decrypts data

\* you can use other connection types like web sockets.

`SSH` can mean either:
- protocol
- suite of implementation utilities

Can do:
- Secure file transer
- Remote device management and account control
- Tunneling
- Forwarding TCP ports an X11 connections

---

- [good video](https://www.youtube.com/watch?v=Atbl7D_yPug)
- [also good video](https://www.youtube.com/watch?v=ORcvSkgdA58)

---
- TCP Forwarding?
- X11 connections?

---


> The ssh or secure shell is a network protocol for operating networking services securely over a network. It uses encryption standards to securely connect and login to the remote system.

> It stores a public key in the remote system and private key in the client system. Thes keys are produced as a pair mathematically. When both are applied to a bi-variable function, it will result in a value which will be used to check whether the pair is valid or invalid. This is the simplest explanation possible. To Learn more, please refer to [this page](https://www.hostinger.in/tutorials/ssh-tutorial-how-does-ssh-work?ref=linuxhandbook.com).


## Usage

### Generate ssh key

```
ssh-keygen
```

> It will prompt for a key-location (where the key will be saved) and passphrase (i.e. password). The passphrase is optional.

Use to generate public key for GitHub or Heroku to push/deploy commits without entering password.

If the key-location is `DIR_PATH/keypairforssh`, there will be two files

1.  `DIR_PATH/keypairforssh`
2.  `DIR_PATH/keypairforssh.pub`

The `.pub` is the public key you can share with remote systems. **DO NOT** share the private key.

### Add private key to the key-agent

> When the key pair is created, it justs exists as a set of two files. In order to connect to the remote system, it has to use the private key.

Use this

```bash
ssh-add DIR_PATH/keypairforssh
```

### Connecting to remote host via SSH

username should be a valid user on the remote system and hostname is DNS-recognizable or an IP address so that ssh can contact the remote system and request for connection.

```bash
ssh [username]@hostname
```

This uses the private key on the local system and public key on the 
remote system and verifies these are valid pairs. It allows login if and only if key pair is valid and spawns a shell (type depends on the configuration for the user on the remote system) for your use. You can use the remote system as you are using the local system.


If the private key is **not** added to the key agent:

```bash
ssh -i /path/to/private/key/file username@hostname
```

## The silly stuff

### Copying files

`scp` is the thing. It works like `ssh` and requires key-pair to work.

```
scp SOURCE_DIR_PATH DESTINATION_DIR_PATH
scp ~/Documents/source.txt [username]@[hostname]:~/Documents
```


### Mounting remote filesystem 😳

> A better way to describe "mount" is "attach".
> 
> The filesystem being mounted is attached to an empty directory of the existing filesystem. That is, the top level directory of the mounted filesystem becomes the directory on the existing filesystem.
> 
> Subdirectories of the mounted filesystem become the subdirectories of the former directory on the existing filesystem, and so on.
>
> (The directory that was mounted on doesn't really have to be empty, but after mounting any contents it had are inaccessible, until the filesystem is unmounted).
— [SO](https://stackoverflow.com/a/29446865)


`sshfs` is the tool for this

```bash
sshfs name@server:/path/to/remote/folder /path/to/local/mount/point
```

`name` is the username accepted on remote system and `server` is the remote hostname.

> The nohup command allows you to keep on running commands even after you disconnect your SSH connection.

[SSH Basics](https://linuxhandbook.com/ssh-basics/)
