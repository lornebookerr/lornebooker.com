---
layout: post
title: External Mail Warnings
subtitle: Setup and Configuration on Exchange Online
tags: [Microsoft 365, Exchange, Cybersecurity]
comments: true
mathjax: true
author: Lorne Booker
---

If we're being honest, emails are a minefield. Teachers can receive all sorts of emails, ranging from genuine requests from parents to someone pretending to be the Head asking for Amazon vouchers. It's our job to help staff clock the difference before they click something that turns into an IT headache.

Tagging emails that come from outside the school is a quick win and makes people stop and think. There are a couple of easy ways to do this, if you're using Exchange Online.

# External Tagging
The easiest and most straightforward way to tag emails is using a handy little switch that Microsoft gives us. You may not know about this option as it's hidden away in the Exchange PowerShell library.

To start, make sure you have access to an elevated Microsoft 365 account. (*I've only tried enabling this using a Global Admin account so can't comment on what permissions are required.*)

1. Open up PowerShell and connect to Exchange Online
```powershell
Connect-ExchangeOnline
```
2. Run the ExternalInOutlook command to enable to tag
```powershell
Set-ExternalInOutlook -Enabled $true
```

That's it! Any emails from outside your organisation will now be tagged with "External", as the example below shows. Subtle, but effective.

![External Tag](https://github.com/lornebookerr/lornebooker.com/blob/main/assets/img/posts/externalmail.png)

Let's say you're part of a MAT, or use an external helpdesk system that doesn't necessarily have the same domain as you; domains can be easily whitelisted from this tag using the same PowerShell library. 

To whitelist an entire domain, you can use the "Allowlist" switch.
```powershell
Set-ExternalInOutlook -AllowList *@exampletrust.org
```

There may be multiple domains that you wish to add to this list, you can amend your allowlist by wrapping the domains into an array.
```powershell
Set-ExternalInOutlook -AllowList @{Add="*@exampletrust.org"; Remove="user@example.org"}
```
# Big Red Banner
If you want something a bit more in-your-face, a bright red warning at the top of the email is the second method that I would recommend.

1. Start by opening up the Exchange Online Admin Center
2. Go to Mail Flow > Rules
3. Click the "+" and choose "Create a new rule…"
4. Call it something of your choosing, for example: "External Email Warning"
5. Under "Apply this rule if…", choose "The sender is located…" > "Outside the organisation"
6. Click "More options"
7. Under "Do the following…", choose "Apply a disclaimer" > "Prepend a disclaimer"
8. Paste in your HTML (example below)
9. Choose Wrap and click OK

Here's an example of what I put at the top of external emails.

![The Big Red Banner](https://github.com/lornebookerr/lornebooker.com/blob/main/assets/img/posts/bigredbanner.png)

```html
<div style="padding-top: 2px; padding-left: 5px; padding-right: 5px; padding-bottom: 5px;">
    <p style="font-size: small; padding: 5px; text-align: center; border-style: dashed; border-radius: 5px; border-color: red; color: red;">
        <u><b>EMAIL SECURITY REMINDER</b></u><br>  
        This email came from outside your organisation. Don’t open attachments or click links unless you're absolutely sure it’s safe.<br>
        <b>If in doubt, give IT a shout.</b>
    </p>
</div>
```

# Conclusion

Whether you go for the subtle tag or the full-blown red warning banner, both will *(hopefully)* make your staff stop and think before they open up any dodgy emails. It’s a tiny change that can save you hours of fixing things when someone clicks the wrong thing.
