---
layout: post
title: "Apache vs Nginx vs Caddy"
subtitle: "Comparing Apache, Nginx, and Caddy for self-hosting a webserver over a local network"
tags: Blog, homelab security, thermostat, knowledgedump, adventure
---

## Introduction

In a recent blog post, I shared everything I am learning while I add HTTPS to the self-hosted servers in my home. Since that post, I have tested quite a few different webservers and learned a lot more about dealing with self-signed certificates on a local network. I wanted to share my findings.

So far, I have tested Apache, Nginx, and Caddy. This post is from a hobbyist homelab perspective. I can only speak about the experience of configuring and using these servers with a use-case of having basically one user over an local area network. I cannot provide any insights on how secure or reliable these servers will be out in the wild.

## Apache

Back when I started making my DIY thermostat project and decided the best way to implement a GUI for it would be a webapp, I first thought of Apache. In my experience so far, Apache is the quickest and simplest way to spin up a serve a static webapp. It's as simple as 3 easy steps:
1. Install Apache
2. Add some HTML to the `/var/www/html` directory
3. Open your browser and navigate to your new website

That's it. It's actually just that easy.

Adding HTTPS also wasn't very difficult. Once I had worked out how to use openssl to generate the certificate and key, I just had to create a simple config to tell Apache where the cert and key files are. Easy.

Eventually, I learned about reverse proxies. The brains of my DIY Thermostat project uses FastAPI, and when I first started using Apache, FastAPI had full HTTPS support. I recently updated to the latest version of FastAPI to discover that HTTPS is no longer supported, and if you want to add encryption, a reverse proxy is the recommended solution.

## Nginx

Learning that I needed a reverse proxy lead me to discovering Nginx. As I went through this adventure to encrypt all the things, I started finding that just about every webserver in my network either did not support HTTPS or had a big disclaimer in the docs about how they don't recommend using their built-in HTTPS and they recommend using a reverse proxy instead. So I started implementing Nginx as a reverse proxy for these servers.

My biggest complaint about Nginx is that the config files are hard to understand. On some part this is a good thing. Nginx's configs are hard to understand because it's so powerful. It has tons of features and can do so much. As I understand it, as well, it's very well hardened and incredibly reliable. Luckily, it's so widely used that there's tons of tutorials and examples on the internet. This makes it really easy to find a working example that can be easily copied and pasted. I just wish these examples came with more documentation on why or how they worked. All of my Nginx configs have quite a few lines that I don't know what they do, I just know it doesn't work if they're not there.

This also created an issue whenever things went wrong. I am struggling right now with getting a reverse proxy to work with my ebook server, Kavita. For some reason, certain requests are going to the wrong port and without understanding the details of how the example config works, I am struggling to determine whether the issue is with the Nginx or Kavita. It also might be an issue with my web browser.

## Caddy

When Caddy popped up in my research for this project, it really caught my eye. Not only does Caddy claim to do basically everything I'm trying to do. It also claims to do a lot of it automatically and with a very simple configuration. I mentioned in my last post about how frustrating it has been to deal with self-signed certs. Since there isn't a trusted certificate authority in my local network, all of the devices I use to connect to these servers complain. Some of them outright refuse. That's where Caddy comes in.

Something that makes Caddy really fun is that it generates self-signed certs automatically. I don't have to fiddle with extremely long `openssl` commands. It will also autorenew the certs which means I don't have to remember to do that myself. The thing that stands out the most for me, is that it generates a root cert. This means that I can tell all my devices to trust Caddy's root cert and that device will not trust all the certs served by this Caddy server. That solves most of my problems right there.

One relatively minor issue I'm facing is that, to my knowledge, Caddy does not currently have a way to specify a custom DNS server. This isn't an issue in most situations, but it happens to be an issue for my specific use-case. I am running basically everything in docker containers. I like that it theoretically adds additional security. If someone finds a way to inject commands, they're limited by being in a container and will have a harder time getting root permissions. I also like that I can utilize docker's bridge networks to hide the unencrypted HTTP server and make it so the only server that can communicate with it is the reverse proxy. In order to do this, though, I need to tell Caddy to use docker's internal DNS server. Without this ability, I think my only option is to expose everything and use a firewall on the server to block the unencrypted servers from talking outside.

Honestly, I should be using a firewall anyway, so maybe that limitation isn't too big.

## Conclusion

Apache is perfect for a specific use-case. You have a static webpage that you want to server, and you want the setup to be quick and easy. Nginx is complicated and difficult to understand, but there are so many exmaples on the internet that you don't necessarily have to understand it to use it. Caddy is super easy to get started and very powerful when it comes to implementing HTTPS for static pages or reverse proxies, but it has some limitations with docker networks.

As of the time of writing this post, I'm mainly using Nginx for my servers, but I plan to continue poking around with Caddy to see if I can find a workaround. I realized as I was writing this that there may to specificy a static IP for each docker container, and doing so would eliminate the need for Caddy to use docker's DNS.
