---
title: "HTML e-mail best practices"
---

Ok, poor boy (or girl). Now you'd like to learn some useful tips to create beautiful HTML e-mails using whole CSS goodness. Sorry to disappoint you, but you can't. At least if you want to be cross-client and I'm sure you do.

### CSS support in mail clients

CSS support in mail clients, either desktop or web, really sucks. There are no standards. Everyone do it in his own way.

Poor CSS support in Microsoft Outlook 2007, Windows Live Hotmail, Google Gmail, Lotus Notes 8, Eudora and plenty more. See yourself, take a look at [CSS support in email clients report](http://www.campaignmonitor.com/css/).

HTML/CSS support in MS Outlook 2007 is surprisingly much worse than in MS Outlook 2003. Microsoft seems not to like webdevelopers much while they perfectly show how to use mere 4 years to break their software even more. It stopped supporting most CSS styles, e.g. *height*, *width* or *vertical-align*, which are supported by other clients.

Hopefully [Email Standards Project](http://www.email-standards.org) will manage to change it.

For now we're in the doghouse.

### Golden rule: keep things extremely simple

Forget web standards. E-mail is NOT website.

Use tables for layouts. Many tables better than one complex table.

Max width 580px.

Avoid styles in STYLE tag. Use inline styles instead. Some webmails (Gmail, Hotmail) completely strip both HEAD and STYLE tags.

Poor support for CSS selectors.

Relatively safe-to-use CSS styles, which are supported by nearly all mail clients (apart from Eudora which gracefuly does not support anything) are *background-color*, *border*, *color*, *display*, *font-family*, *font-size*, *font-style*, *font-weight*, *font-variant*, *letter-spacing*, *text-align*, *text-decoration*, *text-indent*. 

Be careful with images. 

* [Mail clients often block images by default](http://www.campaignmonitor.com/blog/archives/2007/02/current_conditions_and_best_pr_1.html).
* Do not forget about ALT attribute.
* Use captions for important images.

Send both HTML and plain text version together.

That's it. Pretty restrictive, huh? Still, as always, you can try to be creative like [Campaign Monitor](http://www.campaignmonitor.com/templates/) or [MailChimp](http://www.mailchimp.com/resources/html_email_templates/) guys.

Check following links for more info:

* [www.sitepoint.com/article/code-html-email-newsletters/](http://www.sitepoint.com/article/code-html-email-newsletters)
* [www.sitepoint.com/article/designers-guide-html-email/](http://www.sitepoint.com/article/designers-guide-html-email)
* [www.anandgraves.com/html-email-guide](http://www.anandgraves.com/html-email-guide)

