---
title: "Logging on to the cluster"
layout: archive
permalink: /logging_on/
---

Typically when working in bioinformatics we would log into a computing cluster such as the one we have prepared for this course. Those clusters provide computational resources  much more powerful than the average personal computers, allowing us to run highly complex computational tasks required by some bioinformatics analyses. When you are back in your home institutions, you can also likely use a similar method to log into the computing nodes there. How do we log in for this course? 


### Mac OS X and Linux users

#### Logging on

If you are using a Mac or Linux machine, you will need to open a `terminal` window and then type `ssh`.

`ssh` stands for [secure shell](https://en.wikipedia.org/wiki/Secure_Shell) and is a way of interacting with remote servers. You will need to log in to the cluster using both `ssh` and a keyfile that has been generated for you.

Firstly, download the keyfile and open a terminal window. Then copy it into your home directory like so:

```shell
cp mark.pem ~
```
Then you should be able to log in with `ssh` whatever your working directory is. You need to provide `ssh` with the path to your key, which you can do with the `-i` flag. This basically points to your identity file or keyfile. For example:

```shell
ssh -i "~/mark.pem" mark@54.245.175.86
```

Of course you will need to change the log in credentials shown here (i.e. the username and keyfile name) with your own. **Also be aware that the cluster IP address will change everyday**. We will update you on this each day.

You might be prompted to accept an RSA key - if so just type yes and you will log in to the cluster!

#### Downloading and uploading files

Occassionally, we will need to shift files between the cluster and our local machines. To do this, we can use a command utility called `scp` or [secure copy](https://en.wikipedia.org/wiki/Secure_copy). It works in a similar way to `ssh`. Let's try making a dummyfile in our local home directory and then uploading it to our home directory on the cluster.

```shell
# make a file
touch test_file
# upload to cluster
scp -i "~/mark.pem" test_file mark@54.245.175.86:~/
```
Just to break this down a little we are simply copying a file, `test_file` in this case to the cluster. After the `:` symbol, we are specifying where on the cluster we are placing the file, here we use `~/` to specify the home directory.

Copying files back on to our local machine is just as straightforward. You can do that like so:

```shell
# download to local
scp -i "~/mark.pem" mark@54.245.175.86:~/test_file ./
```
Where here all we did was use `scp` with cluster address first and the the location (our working directory) second - i.e. `./`

#### Making life a bit easier

If you are logging in and copying from a cluster regularly, it is sometimes good to use an `ssh` alias. Because the cluster IP address changes everyday, we will not be using these during the course. However, if you would like some information on how to set them up, see [here](https://markravinet.github.io/CEES_tips_&_tricks.html)

### Windows users

#### Logging on

If you are using a Windows machine, you will need to log on using [PuTTY](https://www.putty.org/) since there is no native `ssh` client. PuTTY does not natively support the private key format (.pem) needed to login to our Amazon cloud instance. You first need to convert the private key that we gave to you to a key that PuTTY can read. PuTTY has a tool named PuTTYgen, which can convert keys to the required PuTTY format (.ppk). When you installed PuTTY, it will also have installed PuTTYgen.

First, start PuTTYgen (for example, from the Start menu, choose All Programs > PuTTY > PuTTYgen). Then select RSA and click on Load:

![](/images/putty/fig1.png)

In the new window that pops up, Change "PuTTY Private Key Files" to "All Files" to allow you to find your pem file.

![](/images/putty/fig2.png)

Then save your key and click on YES to dismiss the Warning as shown below.

![](/images/putty/fig3.png)

Great, now your key file is ready and we can start Putty. In Putty, enter your user name and the IP address in the format \<user_name\>@\<IP adress\>. Make sure that 22 is given as Port and that SSH is selected.

![](/images/putty/fig4.png)

Next, on the menu on the left, expand the "SSH" panel by clicking on the + and select "Auth". Then, select your new putty format key file (.ppk) with Browse. Do NOT click on Open yet.

![](/images/putty/fig5.png)

To make sure that you will not get logged out if you are inactive for a while, you can configure PuTTY to automatically send 'keepalive' signals at regular intervals to keep the session active. Click on Connection, then insert 180 to send keepalive signals every 3 minutes. Do NOT click on Open yet.

![](/images/putty/fig6.png)

To avoid having to change the settings each time you log in, you can save the PuTTY session. Click on Session to get back to the basic options window. Once you are happy with all settings, give the session a name and click Save. Now you are ready to start the session with "Open". The first time PuTTY will display a security alert dialog box that asks whether you trust the host you are connecting to. Click yes.

![](/images/putty/fig7.png)

When you log in the next time, you can just click on the saved session and click load. If the IP address changed in the mean time (e.g. because we stopped the Amazon instance over night), you will need to replace the IP address by the new one. I would then recommend to Save the changed settings. Then simply click Open to start the session.

![](/images/putty/fig8.png)

If the IP address did not change and you just want to login again, you can also right-click on the putty symbol in the taskbar (provided that you have pinned it to the taskbar) and select the session.

![](/images/putty/fig9.png)

#### Downloading and uploading files with Filezilla

Filezilla is a handy software to move files from a remote server such as the Amazon cloud or a cluster of your university.

Open Filezilla and choose Edit -> Settings.

![](/images/putty/fig10.png)

Next, choose SFTP and Add the .pem key file as indicated below and click OK.

![](/images/putty/fig11.png)

Finally, enter the IP address and the user name and when you hit enter, it should connect you. Next, time, you can use the Quickconnect dropdown menu, provided the IP address has not changed in the meantime.

![](/images/putty/fig12.png)

Now you will see the file directory system (folders) on your local computer on the left and your folders on the amazon cloud on the right. You can now just drag and drop files from one side to the other.
