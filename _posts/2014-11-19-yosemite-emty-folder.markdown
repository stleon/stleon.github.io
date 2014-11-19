---
layout: post
title:  "Mac OS X 10.10 Yosemite. Пустая папка в Launchpad"
description: "Баг в Launchpad: когда папка становится пустой, она не исчезает"
date:   2014-11-19 09:51:00
categories: apple yosemite
permalink: /blog/yosemite-emty-folder
---

Так бывает, что папка в **Launchpad**, вдруг, становится пустой:

![Пустая папка в Launchpad](/downloads/empty_folder.png)

Но она должна автоматически исчезнуть. Если этого не произошло, то вам под кат.
<!--more-->

{% highlight text%}
defaults write com.apple.dock ResetLaunchPad -bool true
killall Dock
defaults write com.apple.dock ResetLaunchPad -bool false
{% endhighlight %}

Для тех, кто не понял - открываем **Терминал**, вводим по одной строчке, нажимая **Enter**. 
