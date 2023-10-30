---
title: Mail Configuration
description: 
published: true
date: 2023-07-08T21:51:17.053Z
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

## A note on port 465
Do not use port 465. Your mail server should also allow connecting to port 587, use that instead. `secure` should be set to `tls` in `core/email.php`.

If you have to use port 465, and emails are not being delivered/timing out, try setting `secure` to `ssl` in `core/email.php`.

This port is obsolete, as described in [RFC 8314](https://www.rfc-editor.org/rfc/rfc8314.html#section-7.3):
> Historically, port 465 was briefly registered as the "smtps" port. This registration made no sense, as the SMTP transport MX infrastructure has no way to specify a port, so port 25 is always used.  As a result, the registration was revoked and was subsequently reassigned to a different service.  In hindsight, the "smtps" registration should have been renamed or reserved rather than revoked.  Unfortunately, some widely deployed mail software interpreted "smtps" as "submissions" [RFC6409] and used that port for email submission by default when an end user requested security during account setup.  If a new port is assigned for the submissions service, either (a) email software will continue with unregistered use of port 465 (leaving the port registry inaccurate relative to de facto practice and wasting a well-known port) or (b) confusion between the de facto and registered ports will cause harmful interoperability problems that will deter the use of TLS for Message Submission.  The authors of this document believe that both of these outcomes are less desirable than a "wart" in the registry documenting real-world usage of a port for two purposes. Although STARTTLS on port 587 has been deployed, it has not replaced the deployed use of Implicit TLS submission on port 465.

> [Source](https://docs.namelessmc.com/en/smtp) on where we got most of the info
{.is-info}