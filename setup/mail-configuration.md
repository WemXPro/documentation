---
title: Mail Configuration
description: 
published: true
date: 2023-11-07T11:46:51.330Z
tags: 
editor: markdown
dateCreated: 2023-03-09T12:41:18.157Z
---

# Using Internal Mailer
PHP comes with an internal mailer which lets you send emails from the server. It's very easy and quick to set up.

```shell
cd /var/www/wemx
php artisan setup:mail
```
2. Set driver to "mail", origin to `no-reply@YourDomain.com` and encryption method to TLS
3. Go to Admin area -> Billing -> Emails -> Click Compose Email; Send yourself a test email, if it arrives, you're done.
4. If you don't receive the test email in step 3; run the commands below:
- `sudo apt-get install sendmail`
- `sudo sendmailconfig`
- Restart Webserver: `sudo service nginx restart` OR `sudo service apache2 restart` 
5. Go back to Billing Email settings, are try sending yourself a test mail.

# Using Custom SMTP Server
```shell
cd /var/www/wemx
php artisan setup:mail
```
2. Set Driver to "smtp"
3. See Information below

## Table {.tabset}

### Gmail
- Please note that you can only send emails using the gmail address, not any other address!
- The username is your Gmail address
- The password is your Gmail Application Specific Password
	- See the note below about how and where to set this up
- The name field can be set to anything you like
- The host is `smtp.gmail.com`
- Port should be set to `587`
- [Enable the Less secure apps setting](https://myaccount.google.com/lesssecureapps?pli=1)
- (Not always required) [Allow access via the captcha](https://accounts.google.com/b/0/DisplayUnlockCaptcha)

> Something to note for Gmail accounts: if you do not have 2FA enabled, it will make you do the Display Unlock Captcha every couple of days. To get around this, enable 2FA and [generate an Application Specific Password](https://support.google.com/accounts/answer/185833?hl=en).
{.is-info}

### Outlook / Hotmail / Live
- Please note that you can only send emails using the outlook address, not any other address!
- The username is your email address (e.g. dave@outlook.com or kevin@hotmail.fr)
- The password is your email account password
- The name field can be set to anything you like
- The host is `smtp-mail.outlook.com`
- The port is `587`

### Other providers
- The username is the email address from your SMTP server or a provided username
- The password is the password associated with your SMTP email address
- The name field should be your website name (though you can put anything here)
- The host is the hostname of your SMTP server, such as your SMTP server's IP address

## After entering the details
> You can check the connection here: `example.com/admin/settings/mail`
{.is-success}

---

> [Source](https://docs.namelessmc.com/en/smtp) on where we got most of the info
{.is-info}