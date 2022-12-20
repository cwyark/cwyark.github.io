---
title: "Configure Your Obsidian Publish with Custom Domain over Google SSL"
header:
    overlay_image: https://images.unsplash.com/photo-1618060932014-4deda4932554?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80
    caption: "Photo by [FLY:D](https://unsplash.com/@flyd2069) on [Unsplash](https://unsplash.com)"
tagline: "Protect your obsidian publush and make a better SEO"
category: obsidian
tags:
- obsidian
- obsidian-publish
---

Do you use [obsidian](https://obsidian.md/)? If yes, you probably heard about [obsidian publish](https://obsidian.md/publish). It is web service where you can upload your obsidian notes to the internet, and let other people have more chance to reach your ideas and your works. Because of it's publicity, this service helps your audience reach you but lacking SEO, it's hard to scale up your engagements. 

<!--more-->

Obsidian publish give each creators a common domain name: `https://publish.obsidian.md/<note name>`. Such domain name is not easy to do SEO.

So you may need to do two extra things:
- Use your custom domain name.
- Set up a SSL certificate.

<i class="far fa-sticky-note"></i> **Note:** Obsidian publsh doesn't allow SSL certificate on custom domain.
  {: .notice--info}
  {: .text-justify}

This tutorial is going to teach you a way to let your **obsidian publish** be under custom domain and protected by the strength of Google cloud.

Here's the diagram for this system.

## 🔛 Set Up a Nginx Proxy

- Go to Google Cloud admin page and create a new project.
- Initiate a google compute engine and start a nginx proxy.

<i class="far fa-sticky-note"></i> **Warning:** You may need to input your billing method when creating a google compute engine because it is not free. Google will charge you when your website traffic reach a certain level.
  {: .notice--warning}
  {: .text-justify}



## 🔐 Enable Google SSL Balancer

- Go to Google Cloud and navigate to `google SSL balancer`
- Create a balancer and point to the `nginx proxy` you just created.

## 🆙 Set Your Custom Domain

This step is pretty straightforward and official website has gave you a similiar [tutorial](https://help.obsidian.md/Obsidian+Publish/Set+up+a+custom+domain).

- Use your gmail (or google workspace) account to register a domain in [Google Domain](https://domains.google.com/)
- Point your domain to the `Google SSL Balancer` You just created.


Now, open your browser and enter to this custom domain. It works!