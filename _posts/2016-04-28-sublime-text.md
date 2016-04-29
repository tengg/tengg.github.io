---
layout: post
title: Sublime Text 3 Introduction and Customization
---

Sublime Text is a perfect editor for code. It is developped in Python, and available for OS X, Linux and Windows.

In this article, I've outlined the features needed to make the most out of using Sublime Text 3. Some advanced customizations are also introduced which saved me a great deal of time every day.

##Why Sublime Text other than ...?

There are handful of good editors available including the Church of Emacs and Cult of vi. The reason I chose Sublime Text is its smooth learning curve and powerful extensibility.

##Installation

Installing of the software is presumably known to everyone and hence ignored.

The next step is install "Package Control", which is the first plugin you'll need in Sublime Text. It's like the "apt-get" or "yum" as in Linux that installs every other plugins.

**However, in some certain network environment (e.g. isolated corporate network), http proxy must be configured first.**

1. In Sublime Text, click the `Preference > Settings - User`.
2. Input the network proxy in the opened tab.

    "http_proxy": "URL_OF_PROXY",
    "https_proxy": "URL_OF_PROXY"


3. Open Sublime Text console. The console is accessed via the ctrl+\` shortcut or the `View > Show Console` menu. Once open, paste the appropriate Python code for your version of Sublime Text into the console.

```python
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
