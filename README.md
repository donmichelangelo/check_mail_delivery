# check_mail_delivery
A Nagios/Icinga plugin that sends an email to a mail server to check via Pop3 if it has been received afterwards.<br>
Very handy if you want to check if the mails of your server may arrive the Inbox of the major mailproviders (or actually any other provider)!
<br>
<br>
Comes with presets for the following profiles:
- Hotmail/Outlook.com
- Gmail
- Yahoo
- GMX
- T-Online
<br>
<br>

However, you are free to add additional profiles. You'll just have to extend the profile list under "my %PROFILES" by adding those directly within the script itself. Before you can use this script, you'll also have to configure this script to use your own provider login credentials!

<br>

**Usage to check the regular mail deliverability:**
```
check_mail_delivery_hosters <provider>
```
**Example:**

```
./check_mail_delivery hotmail
Test Mail O9cOfgQ295f was found in test.john.doe@outlook.com's Inbox
```
<br>
<br>

**Usage to check the mail deliverability from Postmulti Instances:**
```
check_mail_delivery <provider> <postmulti_instance>
```
**Example:**
```
./check_mail_delivery hotmail postfix-mailout
Test Mail B4fUdmH035e was found in test.john.doe@outlook.com's Inbox
```
<br>
<br>

**Three reasons when you should receive the following status message:**
```
Did NOT found Test Mail W1sUugE877n in test.john.doe@outlook.com's Inbox.
Maybe you've been blocked or the timeout (30s) is too low.
```
<br>


1. The IP from which you are sending your mail has been blocked by the provider of your recipient's mailbox.
<br>Most of the major providers do provide a form which you can fill out to request a delisting. Before you request a delisting you should also make sure that your server hasn't sent any junk which could have led to the block. Otherwise you are strongly encouraged to fix the cause of your block __before__ you request a delisting!
<br>
<br>
2. Your mail has been accepted by the provider, but it has been for some reason delivered to the Junk/Spam folder of the recipient's mailbox. __check_mail_delivery__ doesn't check if the mail has been delivered to the Junk/Spam folders, but Inbox! In most cases it's sufficient to login into the provider's Webmail and mark the falsely detected test message as no spam. Future emails should afterwards arrive the Inbox as expected.
<br>
<br>
3. As this status message also states, your timeouts could be too low. __check_mail_delivery__ checks the Inbox of the destination Mailbox after 30 seconds (default value). You may have to increase this value and also the Nagios/Icinga timeouts until you achieve a good and successfull POP3 crawl-rate, though a timeout of 30 seconds has proved to be actually a good working value.

<br>
<br>
<br>
**Additional Notes**
<br>
If you are __NOT__ executing this script under a Red Hat/CentOS alike distribution __OR__ you don't have the package "nagios-plugins-all" installed, you may want to comment out the following lines in this script:
```
use lib "/usr/lib64/nagios/plugins";
```
and:
```
# die with an error if we're not on Linux
if ($^O ne 'linux') {
    print "This plugin only applicable on Linux.\n";
    exit $ERRORS{'UNKNOWN'};
}
```

These lines are not elemental to run this script under Linux Distributions!


