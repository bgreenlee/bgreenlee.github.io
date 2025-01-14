---
title: "Migrating GitHub Pages to Cloudflare"
date: 2025-01-14 07:30:00 -0800
layout: post
tags: github cloudflare
---

Or, On Becoming a Cloudflare Fanboy.

## What's wrong with GitHub Pages?

I'd been using GitHub Pages for hosting static sites, including this one, for years. It's easy and free-ish (I believe they're free for public repos, and if you're paying $4/mo, private repos as well).

One thing you don't get, though, is any kind of visitor metrics. I know I can get this if I add something like Google Analytics to my sites, but I'm loathe to do that for a number of reasons. For one, to be [GDPR compliant](https://gdpr.eu/cookies/), I'd have to add one of those annoying cookie banners to all my sites. Also, ad blockers can block the GA Javascript and skew results. Finally, I don't want to have to add the GA code to every site.

If I were hosting the sites myself, I could get the information I need from the web server logs, but if GitHub is hosting the content, they have that info—but they aren't sharing it.

## Enter Cloudflare Pages

I'd been using Cloudflare for a while, as early on it was the only way to enable SSL on your GitHub Pages sites (GH now provides SSL for all Pages). But I didn't know they had their own ["Pages"](https://pages.cloudflare.com) offering.

Turns out it's super easy to use: just point it to your GitHub repo, tell it what branch you want to deploy from and if there's any build commands you want to run (plus other optional settings like output directory, environment variables,etc.), and it will publish your site on a `pages.dev` subdomain. Then you can add your own domain name via a CNAME record to that subdomain. If Cloudflare is hosting your DNS it will even update your records for you. And everything gets SSL.

The analytics it gives you is decent: total requests, unique visitors, bandwidth, % cached, requests by country/region, etc. You can even see breakdown by source IP, browsers, OS, desktop vs mobile (although oddly it's under the Security section, not Analytics).

One thing that seems to be missing is a breakdown of traffic by page. It would be nice to see, for example, if particular blog posts are getting more traffic than others, but I'm not seeing that data anywhere.

## Going All-In

After moving a few of my sites over and being generally impressed with the Cloudflare experience, I decided to go all-in and consolidate all my domains (more than 20...yes, I have a domain problem) under Cloudflare. This meant having them be my registrar, DNS, and, where appropriate, hosting the content. Previously my domains were spread out among a few different registrars, and my DNS split between registrars, Digital Ocean, and Cloudflare. Having everything under one roof made a lot of sense.

The process was pretty straightforward. The only real snag I hit is that by default Cloudflare sets up "Flexible" SSL/TLS, where traffic is encrypted between visitors and Cloudflare, and then connections to your backend are made over HTTP. But if you already have a cert on your backend, and that backend is configured to redirect HTTP traffic to HTTPS, you'll end up with a redirect loop. The fix is to just go into Cloudflare and change the SSL/TLS mode to "Full".

## So What Does This Cost?

Nothing. It's all free...which has been bugging me a bit. Other than registrar fees when transferring domains to them, I haven't paid them anything. They do have [paid plans](https://www.cloudflare.com/plans/), of course, but the additional features offered—mostly around security—are targeted towards much bigger sites (bot detection, image optimization, cache analytics, web application firewall, etc.). I would actually be happy to pay them _something_, but I haven't hit anything yet to warrant it. However, it seems like the paid plans are all _per domain_, so if I did end up needing to pay for something across all my sites, it would get unreasonably expensive (their cheapest paid plan, "Pro," is $20/mo/domain).

## Fingers Crossed

I do feel a little nervous about going all-in on Cloudflare, as I would going all-in with any company/service, but I've been really impressed with them so far. They've had some [controveries](https://en.wikipedia.org/wiki/Cloudflare#Controversies) in the past, but generally seem like a pretty good company, both technically and ethically. I'll certainly keep you posted if anything changes.