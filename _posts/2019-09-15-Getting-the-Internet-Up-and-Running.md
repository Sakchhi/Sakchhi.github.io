---
layout: post
title: "Getting my wifi to work on Lenovo Ideapad 330 on Ubuntu 18.04"
date: 2019-09-15
---

On Lenovo Ideapad 330, on a fresh install of Ubuntu 18.10 the wifi is disabled.
```
lspci -knn | grep Net -A3; rfkill list
```
shows
```
Wireless LAN
Soft blocked: no
Hard blocked: yes
```

The wireless is blocked by a platform ideapad driver. Running the following command and rebooting would solve this issue.
```
sudo tee /etc/modprobe.d/blacklist-ideapad.conf <<< "blacklist ideapad_laptop"
```
