---
layout: post
title: xcode插件管理
excerpt: "一些实用的xcode插件及其安装方式"
modified: 2013-05-31
categories: articles
tags: [objective c]
comments: true
share: true
author: foxsofter
---

#### [包管理插件alcatraz](http://alcatraz.io)

Install
Paste this into your terminal:

curl -fsSL https://raw.github.com/supermarin/Alcatraz/master/Scripts/install.sh | sh
Alcatraz is available for OSX 10.9 and Xcode 5+ only.

Uninstall
Delete the plugin:

rm -rf ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins/Alcatraz.xcplugin
Remove all cached data:
rm -rf ~/Library/Application\ Support/Alcatraz
装了这个之后可以搜索并安装下面这些插件。

#### ACCodeSnippetRepositoryPlugin

codesnippet插件

#### backlight

高亮当前行

#### GitDiff

#### FuzzyAutocomplete

自动补全

#### [CocoaPods](http://cocoapods.org/)

安装CocoaPods
sudo gem install cocoapods
如果墙的存在，可以参考这个地址安装
http://code4app.com/article/cocoapods-install-usage
