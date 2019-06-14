---
layout: post
title: "Build a custom email with Mailgun & Namecheap and manage it from Gmail"
categories: blog
published: true
permalink: /:categories/build-custom-email-with-mailgun-and-namecheap/
---

![title_logo](https://user-images.githubusercontent.com/9669739/59276449-478fc480-8c99-11e9-862a-3759cd4c75a5.png)

I've used Zoho Mail for quite some time, but was never happy with their UI. With some googling I found that [Mailgun](https://www.mailgun.com) makes it pretty easy for me to set up a custom email that I can manage on Gmail with my own domain from [Namecheap](https://www.namecheap.com) for free!

In this tutorial we'll be going over how I created my own personal email (`atsushi@yatsushi.com`) from the Namecheap domain I purchased (`yatsushi.com`). How it will work is that we will create a new receiving and sending route specifically for our new email which will then be forwarded to existing Gmail address for us to read. From there we'll add a new account on Gmail where we can send or reply to a new email straight from Gmail! 

## Registering a domain on Mailgun

First, go to [Mailgun](https://www.mailgun.com) website and make an account. The username and email that you use here doesn't affect this new email we'll be making so no pressure.

Mailgun have free plan for up to 10,000 email as of June 2019, and if you ask me that's more than enough for my personal use.

After you've logged in, Go to **Sending > Domains** from the sidebar. Click on "Add New Domain" from that page (aka [here](https://app.mailgun.com/app/sending/domains/new)).

Here's what it might looks like.

![image](https://user-images.githubusercontent.com/9669739/59275093-a0119280-8c96-11e9-90d9-c298d42e1359.png)

Although they recommend using dedicated subdomain for the email, using the main domain like `yatsushi.com` worked for me.

When you add a new domain the page will jump to DNS setting page. Leave this page in one tab, and open new tab with Namecheap to do the next step simultaneously.

## Adding DNS records

Jump to your domain's DNS setting page on Namecheap. Before we put in any new DNS records, let's check what the Mailgun says to do.

![mailgun_setting](https://user-images.githubusercontent.com/9669739/59277364-0f898100-8c9b-11e9-9820-280573395aaf.png)

(Grayed out my own DNS record values)

**DO NOT copy Mailgun's hostname field into Namecheap's "Host" value!** (I waited 48 hours until I realized I did something wrong :/). Turns out Namecheap expects it's own domain to be written as **@** sign. This means where mailgun expects `mailo._domainkey.example.com` field remove `example.com`, so on Namecheap you would write `mailo_domainkey` only. After you fill it out this way the form should look something like this:

![namecheap_setup_1](https://user-images.githubusercontent.com/9669739/59277981-1a90e100-8c9c-11e9-8e12-01e5153fccf1.png)

Do the same operation for MX Records and CNAME! (MX record's hosts should be **@**, and CNAME record's host field would be writen as `email` instead of what Mailgun says to write which is `email.example.com`)

Sweet! Now that DNS records are added, let's refresh the **Domain Setting** page couples times. Note that Namecheap claims DNS records update sometimes take couple hours to 48 hours (for me it took only couple minutes).

If everything is working on Namecheap side at this point, you should see green check mark along different requirements!

![complete_setup_page](https://user-images.githubusercontent.com/9669739/59281205-d30d5380-8ca1-11e9-92b2-63d610d60a0f.png)

## Setting up a receiving route

We're ready to set up a receiving route on Mailgun. Go to **Receiving** setting page from the sidebar, and click on **Create Route** (aka [here](https://app.mailgun.com/app/receiving/routes/new)). This page is where we make our new email forward to existing gmail address.

I decided to use "Match Receipient" option but "Catch all" should work as well. For the receipient we'll put our new email (`atsushi@yatsushi.com` for me). For the **Forward** section we'll put our existing Gmail address. Here's what it might look like when this part of the page is done.

![receiving](https://user-images.githubusercontent.com/9669739/59509077-faa32c80-8eea-11e9-9773-d825ea71bea2.png)

Set the priority to low number (I use 1), and we're good to go! Click "Create Route", and we'll move on to sending part.

## Setting up a sending route

Go to **Sending > Domain Settings** from the sidemenu. Find **SMTP Credentials** tab header. Once you're in this page, click on New SMTP User. This is where we get to define our new custome email! Very exciting. Go ahead and create new credentials.

![image](https://user-images.githubusercontent.com/9669739/59509729-cdf01480-8eec-11e9-8117-4b3ba87304e6.png)

It will give you a password, and **be sure to store it somewhere safe since they won't show it to you again!**. We'll be using other details included on this page to set up a new account on Gmail.

## Adding a new account on Gmail

Go to Gmail inbox, and go to **Settings** page from the gear icon. Click on **Account and Import** tab, and go to **Add another email address** link from *Send mail as* subsection.

![add_email](https://user-images.githubusercontent.com/9669739/59511920-85d3f080-8ef2-11e9-9815-70e7ca189546.png)

On a new pop up, put your new address into an *email* field then click next. On the next page, we'll put what SMTP Setting page on Mailgun suggested to put. Use **smtp.mailgun.org**, **your new email**, and **previously saved password** in each field. Here's what it may look like.

![smtp](https://user-images.githubusercontent.com/9669739/59510683-48ba2f00-8eef-11e9-8e5a-bba7aa445c1e.png)

When you click **Add Account**, Gmail will send confirmation email to the new email __which should be forwarded to your own Gmail address__! Do the confirmation steps and Gmail is set up to send new emails from your custom domain email!

![new_email](https://user-images.githubusercontent.com/9669739/59511768-1b22b500-8ef2-11e9-82a8-ae2234506627.png)

And that's it! You're email is ready to be used.

Feel free to email me at `atsushi@yatsushi.com` with your brand new email to let me know it works, or if you have any feedback on this post! 