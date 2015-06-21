# Mailgun

It considered bad practise to send mail through your own server instead a lot of people will use some form of SMTP but STMP can be slow and its recommended that you use API from a large provider. We've chosen [Mailgun](http://www.mailgun.com/) as they offer 10,000 emails for free per month which should be enough for most sites.

### Set Up

Sign up for account at [Mailgun](http://www.mailgun.com/). Once signed up you can go Domains -> Add Your Domain. If I was setting one up for `bigbitecreative.com` I would enter into the box: `mg.bigbitecreative.com`. You will be asked to make some changes to DNS, once these are completed you should see this at the top of your settings:

![alt text](http://getsuzie.com/assets/images/docs/mailgun.png "mailgun")

Now lets open the `.env` and you see some mailgun settings:

```
MAILGUN_KEY=key-xxxxxxxxxxxxxxx-xxxx
MAILGUN_DOMAIN=domain.com
MAILGUN_FROM=user@domain.com
```

Lets update them with the info above.

```
MAILGUN_KEY=key-myapikey-here
MAILGUN_DOMAIN=mg.bigbitecreative.com
MAILGUN_FROM=user@mg.bigbitecreative.com
```
And thats it your site will now be using Mailgun to send emails.
