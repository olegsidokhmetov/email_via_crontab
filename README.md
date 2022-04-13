# Setting ssmtp for sending email via crontab

------------



ssmtp
=========

sudo apt-get update
sudo apt-get install ssmtp

Making a backup copy and editing
------------

sudo cp /etc/ssmtp/ssmtp.conf /etc/ssmtp/ssmtp.conf-0
sudo nano /etc/ssmtp/ssmtp.conf

Edit it to the following state
--------------

    root=user@gmail.com
    AuthUser=user@gmail.com
    AuthPass=pass
    FromLineOverride=YES
    mailhub=smtp.gmail.com:587
    UseSTARTTLS=YES


Check
------------

Next, if google mail will be used, you must go to https://www.google.com/settings/security/lesssecureapps and allow the use of insecure applications:

mutt
----------------

Install the mail client on your ubuntu server:
**sudo apt-get install mutt**

Send e-mail
-------

After the installation is finished, try to send the mail:
```bash
mutt -s "test mail" user@gmail.com < /dev/null
```
And check the mail you sent the letter to? It should already be there.

Open the file /etc/ssmtp/revaliases
------------------
and add the line:

```bash
[yourPCUsername]:[yourRealEmail@yahoo.com.au]:smtp.mail.yahoo.com:587
```
That's it, you're good to go! To test, the easiest way (IMO) is to create a file with the following in it:
------------------
and add the line:
    
    To: user@gmail.com
    From: "whateverYaWant" <user@gmail.com>
    Subject: Some Notifying Email
    MIME-Version: 1.0
    Content-Type: text/plain
    
    Body of your email goes here! Hello world!

Finally, run 
----------------
```bash
cat fileWithEmailInIt.txt | sendmail -i -t
```

then wait a few seconds (10-30) and check your email!

crontab -e
----------------

```bash
MAILTO=user@qmail.com
00 16 * * 1,2,3,4,5 /home/scripts/loop_off_servers.sh
00 6 * * 1,2,3,4,5 /home/scripts/loop_on_servers.sh
```

If have some problem...
----------------
sending mail via commandline worked fine, but no mail was sent through crontab. Fixed it by changing ```bash
FromLineOverride to NO 
```in ```bash
/etc/ssmtp/ssmtp.conf
```. During debugging, it was also helpful to add ```bash
Debug=YES
``` to```bash
 ssmtp.conf
``` - it resulted in more detailed information being logged to ```bash
/var/log/mail.log
```. 
