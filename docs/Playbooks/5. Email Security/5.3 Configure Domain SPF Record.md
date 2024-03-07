### Configuring Email Authentication
Your businesses' presence online is anchored by your domain name. You’ve likely spent a lot of resources in building your online reputation, which is why it’s important to maintain and protect it.

A custom-made Sender Policy Framework (SPF) record will reduce the risk of your domain name being spoofed (or fraudulently impersonated), and stop your messages from being designated as spam before they get to your recipients.

Cyber criminals tend to use email spoofing, which is the creation of emails with an illegitimate (or forged) sender address, to socially engineer recipients into believing they are a legitimate sender. This is actually fairly easy for them to do, as many mail servers do not conduct authentication. Spoofed emails are used to distribute spam, false invoice scams and numerous phishing attempts.

To prevent yourself from being spoofed, you can add an SPF record to your Domain Name Service (DNS) zone file. When designed correctly, a valid SPF record will prevent criminals from spoofing your domain.

Do consider however that having an SPF record is not 100% effective, because of the issue that some mail providers won’t actually check for it. While the vast majority of them do, recipients who don’t use SPF frameworks to verify emails are open to the possibility of receiving spoofed emails.

With that in mind, it’s important to ensure that not only is your domain verified with an SPF record, but that your email hosting checks for SPF records well. This way you can prevent your domain from being spoofed, mitigate the possibility of your organisation receiving spoofed emails, and ensure that your emails are arriving in recipients inboxes instead of their spam folder.

We recommend that you send the following email to your IT manager, service provider, or person(s) responsible for your email and DNS:  
 

_Hello,_

_We have been instructed by our cyber security vendor to ensure that we have an active SPF record added to our DNS._

_Can you confirm if this is already in place?_

_If not, kindly have this set up ASAP._

_Thank you._

We recommend that you complete a test before and after configuration using the tools located at the following link:

**MX Toolbox**  
Free online DNS tools and web lookups](https://mxtoolbox.com/)