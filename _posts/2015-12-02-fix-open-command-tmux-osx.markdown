---
layout: post
title:  "Fix for `open` command in tmux on OS X"
date:   2015-12-02 03:35:00
categories: terminalfu tmux osx
---

This was a deceptively difficult problem to solve that I just encountered, so I thought I would document the fix for the likely droves of others out there who might experience similar issues. 

In OS X, a terminal can typically invoke the default program for a given filetype by simply running `open file.jpg`. This works quite well without additional configuration for most common file extensions. Examples include:

- `open file.html` will spawn your default browser with `file.html`
- `open .` spawns a new Finder window at the current working directory
- `open file.jpg` launches Preview displaying your photo

However, this all breaks down when you're running your terminal session within tmux. You'll likely be granted with the following error:

`The window server could not be contacted. open must be run with a user logged in at the console, either as that user or as root.`

Yuck.

Here's the fix...

I was able to wire this together using the [`reattach-to-user-namespace`][reattach] package readily available through `brew`:

Steps to fix are simply:
1. Make sure your brew install is up-to-date: `brew update`
2. Install the necessary package: `brew install reattach-to-user-namespace`
3. Configure tmux to use the new package: 
`echo "set -g default-command \"reattach-to-user-namespace -l ${SHELL}\"" >> ~/.tmux.conf`

Now, altogether!
{% highlight bash %}
brew update
brew install reattach-to-user-namespace
echo "set -g default-command \"reattach-to-user-namespace -l ${SHELL}\"" >> ~/.tmux.conf
{% endhighlight %}

When complete, be sure to close all active tmux sessions:
  `tmux list-sessions` 
followed by:  
  `tmux kill-session <id>`

One quick word of warning: This fix works great, but it did cause an initial hang in Finder (and all running applications), which required a [Force Close of Finder][force-close] to get back in business. Everything seems to be running just fine after that hiccup, but I wouldn't want to overlook this word of warning to others who may plan to use it and have critical (unsaved) documents open.

P.S. For those looking for some extra credit on the OS X `open` command, (such as opening application-specific URLs, (For example, [Dash's][dash] handler `dash://`), here's a great article providing a concise overview: 

http://brettterpstra.com/2014/08/06/shell-tricks-the-os-x-open-command/

Happy tmux-ing!

[reattach]:    http://brewformulas.org/ReattachToUserNamespace
[force-close]: https://discussions.apple.com/thread/5703354
[dash]:        https://kapeli.com/dash
