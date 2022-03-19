# check-mail-if-exist
#Verify email existence via command-line

In facts, this is a pretty old tip, and it is still working today.
There are three steps to do it:
1. Locate or find mail servers of target domain, that is, the domain part of the email address you want to verify.
2. Telnet into the mail server to confirm if server is connectable and working.
3. Send commands into mail server to verify email existence.
Okay, let’s do this.
#Step 1: Locate mail servers
In other words, this is to find MX records of the domain in DNS zone file. If the target domain is serving such email address then it must been served from a mail exchange server, and MX records should be configured to define how to handle in/out emails.
To find MX records, perform this command:
```shell
{
  $ nslookup -type=mx DOMAIN_NAME
}
```

It will display one or several mail servers. We will access one of those mail servers to do the verification.
Let say, we want to verify an email named like ayouubcharef@gmail.com. Then we need to do MX lookup with gmail.com. So the lookup command will be:

![image](https://user-images.githubusercontent.com/46047976/159131081-89735bcb-8c8a-4ef9-84d9-f3ece736395b.png)

Your results might differ from me, but we can just access one of those Google mail servers to do the job.
I have written a detail tutorial on MX lookup using command-line. Check it out if you want to know more about it.
# Step 2: Telnet into mail server
The purpose is to:
verify if mail server is working or not.
to perform email existence verification.
And to do that, we will use telnet utility. You might want to install it if it’s not available on your computer, then proceed the command.
```shell
{
  $ telnet gmail-smtp-in.l.google.com 25
}
```
There are two parameters for the command:
MAIL_SERVER : is the MX records we locate on step 1 previously.
MAIL_PORT : is the port to access mail exchange server. By default, it is 25.
So, let’s take one of the above google mail servers, and telnet into it.

#image

If it works then you should be inside the telnet shell of that server, from here you can send commands to request actions
#Step 3: Send commands to verify email existence
At the moment, you should be inside telnet prompt that has access to MX server. If not, try step 2 again.
Try following actions:
#image

he next command is to specify the sender’s email address, that is MAIL FROM. You can put in any email address.
Now, the important command of our goal, RCPT TO, this is to specify the receiver’s email address, 
and we should type in the email we want to verify on this mail servers. In above example, 
I typed in RCPT TO: <ayouubcharef@gmail.com>, and it responses with 250 OK, it means that email exists on Gmail server.
And if email doesn’t exist on MX servers, something like below will display:

#image

Look at the description, it is very descriptive response from Gmail server. However, for other mail servers, it might not be clear like this. But you will know whether the email exists or not by looking at the response code, 550.
#Error Code
While issuing commands to mail servers via telnet, you just need to confirm the response code.
250 : on succeed.
550 : if fail.
#Important Notes
Several points you need to be aware.
1)Verifying email existence in this method generally works in most of the case. But some mail servers is configured to accept any email in RCPT TO command.
2)Don’t try to do this often, your IP might be put into blacklist.
Especially, when you try to automate this process via code.


