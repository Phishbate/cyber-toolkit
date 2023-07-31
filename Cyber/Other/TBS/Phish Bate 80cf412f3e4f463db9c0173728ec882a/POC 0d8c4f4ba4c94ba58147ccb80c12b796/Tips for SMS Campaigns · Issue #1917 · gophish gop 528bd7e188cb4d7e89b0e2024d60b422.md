# Tips for SMS Campaigns · Issue #1917 · gophish/gophish

[https://github.com/gophish/gophish/issues/1917](https://github.com/gophish/gophish/issues/1917)

### How to Send Gophish SMS Campaigns

I'm not sure where this should go (is there a gophish wiki?), but in case someone else searches for 'SMS' here's my solution:

1. Sign up for an SMTP to SMS service (e.g TextAnywhere, FastSMS, Clicksend)
2. Upload users with an email of `<phonenumber>@<provider>` e.g [44754125474@sms.fastsms.com](mailto:44754125474@sms.fastsms.com)
3. Create a sending template with text only
4. ??
5. SMShing!

e.g:

![Tips%20for%20SMS%20Campaigns%20%C2%B7%20Issue%20#1917%20%C2%B7%20gophish%20gop%20528bd7e188cb4d7e89b0e2024d60b422/88930766-d1229d00-d273-11ea-974d-bcf4bde170b0.png](Tips%20for%20SMS%20Campaigns%20%C2%B7%20Issue%20#1917%20%C2%B7%20gophish%20gop%20528bd7e188cb4d7e89b0e2024d60b422/88930766-d1229d00-d273-11ea-974d-bcf4bde170b0.png)

When the user clicks the link, we get the landing page:

![Tips%20for%20SMS%20Campaigns%20%C2%B7%20Issue%20#1917%20%C2%B7%20gophish%20gop%20528bd7e188cb4d7e89b0e2024d60b422/88931314-91a88080-d274-11ea-9580-163d64b55c56.png](Tips%20for%20SMS%20Campaigns%20%C2%B7%20Issue%20#1917%20%C2%B7%20gophish%20gop%20528bd7e188cb4d7e89b0e2024d60b422/88931314-91a88080-d274-11ea-9580-163d64b55c56.png)

The only downside is that you need duplicate users. [@jordan-wright](https://github.com/jordan-wright) I know you don't want built in SMS support, but maybe a middle ground in the future would be to allow primary and secondary email addresses for users - then when firing off a campaign you could have the option to use the secondary email address (with the phone number details). Just a thought :) Might actually be useful for storing users' home email addresses too.