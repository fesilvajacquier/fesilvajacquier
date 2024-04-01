---
layout: post
title: curl timeout
date: 2023-07-21
category: til
---

## Origin (context)

Opening a terminal tab it would take minutes until the prompt was available (as if it was frozen).

## Exploration journey

I knew that instantiating a terminal session implies executing `zshrc` (the `zsh` run commands). But I had changed nothing in there.

I observed the processes being run when opening a tab. There I found that what was taking too long was a `curl` to `http://whatthecommit.com/index.txt`. This occurs every time I open the terminal because I have defined an alias that fetches an stupid message from this url and uses it as a commit message (`sillycommit="git commit -m \"$(curl -s http://whatthecommit.com/index.txt)\""`).

Also, at the moment of the problem there was a **Heroku** outage

## Result

TIL¹ apparently `whatthecommit` is hosted on **Heroku**.

TIL² it is not smart to temper your setup with _funny_ stuff (note to self: you are a developer, not a comedian).

TIL³ you can pass an option to `curl` to set a timeout (`curl --max-time 1 <url>` where the number is in seconds).

TIL⁴ the default timeout value for `curl` is 2 minutes.

## Links

NA
