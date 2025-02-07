---
layout: post
title: "Apache vs Nginx vs Caddy"
subtitle: "Comparing Apache, Nginx, and Caddy for self-hosting a webserver over a local network"
tags: Blog, homelab security, thermostat, knowledgedump, adventure
---

## Introduction

In a recent blog post, I shared everything I am learning while I add HTTPS to the self-hosted servers in my home. Since that post, I have tested quite a few different webservers and learned a lot more about dealing with self-signed certificates on a local network. I wanted to share my findings.

So far, I have tested Apache, Nginx, and Caddy. I can't speak to how well they handle traffic and security out in the wild. The servers I am hosting pretty much just have one user and are not accessible to the outside world. I will be sharing the experience of setting up these servers, enabling HTTPS via self-signed certificates, and using the server via local network.

## Apache

Back when I started making my DIY thermostat project and decided the best way to implement a GUI for it would be a webapp, I first thought of Apache. In my experience so far, Apache is the quickest and simplest way to spin up a serve a static webapp. It's as simple as 3 easy steps:
1. Install Apache
2. Add some HTML to the `/var/www/html` directory
3. Open your browser and navigate to your new website

That's it. It's actually just that easy.

Adding HTTPS also wasn't very difficult. Once I had worked out how to use openssl to generate the certificate and key, I just had to create a simple config to tell Apache I wanted it to server HTTPS and where to find the cert files. Easy.

## Nginx

Pretty quickly in my adventure to encrypt all the things, I learned that I needed a reverse proxy. The server that allows my thermostat to actually do its job uses FastAPI. FastAPI supported HTTPS once upon a time, but support was dropped in favor of using reverse proxies (which are likely to be more secure anyway). I was using Apache before. As far as I can tell, Apache does not have support to do that. So that's where I discovered Nginx.

Nginx has a learning curve and I found the config files difficult to understand. Luckily, there's so many tutorials on the internet that finding a working config to copy and paste is incredibly easy. Unfortunately, I'm struggling to understand *why* things work when they do. I have been struggling to get a working reverse proxy for my Kavita server. Some things aren't working and I haven't yet figured out why.

## Caddy

When Caddy popped up in my research for this project, it really caught my eye. Caddy claims to do basically everything I'm trying to do. It also claims to do a lot of it automatically and with a very simple configuration. I mentioned in my last post about how frustrating it has been to deal with self-signed certs. Since there isn't a trusted certificate authority in my local network, all of the devices I use to connect to these servers complain. Some of them outright refuse. That's where Caddy comes in.

Something that makes Caddy really fun is that it generates self-signed certs automatically. I don't have to fiddle with extremely long `openssl` commands. It will also autorenew the certs which means I don't have to remember to do that myself. The thing that stands out the most for me, is that it generates a root cert. This means that I can tell all my devices to trust Caddy's root cert and that device will not trust all the certs served by this Caddy server. That solves most of my problems right there.

One relatively minor issue I'm facing is that, to my knowledge, Caddy does not currently have a way to specify a custom DNS server. This isn't an issue in most situations, but it happens to be an issue for my specific use-case. I am running basically everything in docker containers. I like that it theoretically adds additional security. If someone finds a way to inject commands, they're limited by being in a container and will have a harder time getting root permissions. I also like that I can utilize docker's bridge networks to hide the unencrypted HTTP server and make it so the only server that can communicate with it is the reverse proxy. In order to do this, though, I need to tell Caddy to use docker's internal DNS server. Without this ability, I think my only option is to expose everything and use a firewall on the server to block the unencrypted servers from talking outside.

Honestly, I should be using a firewall anyway, so maybe that limitation isn't too big.

## Conclusion

Apache is perfect for a specific use-case. You have a static webpage that you want to server, and you want the setup to be quick and easy. Nginx is complicated and difficult to understand, but it offers the ability to create a reverse proxy. Caddy is super easy to get started and very powerful when it comes to implementing HTTPS for static pages or reverse proxies, but it has some limitations with docker networks.

As of the time of writing this post, I'm mainly using Nginx for my servers, but I plan to continue poking around with Caddy to see if I can find a workaround. I realized as I was writing this that there may to specificy a static IP for each docker container, and doing so would eliminate the need for Caddy to use docker's DNS.
