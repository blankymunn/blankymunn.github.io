---
layout: post
title: "Customizing github blog"
category: dev
tags: github
---

This post is for explanation about customizing my blog.

After you cloned this blog, you can encounter tons of folders and files and probably get lost among them. So, I'm gonna explain what folder or file under '\_app' is for.<br>

# 1. Assets
```
├── assets/
|   └── _scss/
|   |   └── _user.scss
|   |   └── _variable.scss
|   └── themes/curnata/
|   |   └── _common.scss
|	|	└── _variables.scss
```
The folder 'Assets' is consists of .scss files which contains classes and basic configs for the blog(e.g Blog's color schemes, font-size, ...).<br>

## _scss/_user.scss

This file contains user-make classes.<br>
If you wanna use your own classes, you can add yours to this file.
