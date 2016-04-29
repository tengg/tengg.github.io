---
layout: post
title: Sublime Text 3: Use From Behind a Proxy
---

The plugin "Package Control" plays the role in Sublime Text as "apt-get" or "yum" in Linux, but it does not come with Sublime Text. We can install it easily by the guide provided by [packagecontrol.io](https://packagecontrol.io/installation).

However, in some certain network environment(e.g. isolated corporate network), accessing everything outside must go though a proxy, and **Sublime Text does not use the OS global network proxy preferences.** People often get connection timeout when installing the package control by the guide.

Setting up the network proxy inside Sublime Text is outlined in this article.

<!--more-->

1. Open Sublime Text console. The console is accessed via the ctrl+\` shortcut or the `View > Show Console` menu. Once open, past the following python code into the console. The first one is specify your HTTP proxy. Replace the YOUR_PROXY_URL with actual proxy url.

   ```python 
    proxyurl="YOUR_PROXY_URL"
   ```

2. Then paste following codes to install the package control.

   ```python 
    import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler({"http":proxyurl})) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
   ```
   
3. Once finished, we need to configure the same HTTP proxy for the package control. Click the `Preference > Package Settings > Package Control > Settings - User`, then input the network proxy in the opened tab.

   ```json
    "http_proxy"  : "URL_OF_PROXY",
    "https_proxy" : "URL_OF_PROXY"
   ```

4. Cmd + Shift + P (Ctrl instead of Cmd on Windows and Linux) to activate command palette. Type "install" then enter to start the package control. Then you can install whatever plugins or color schemes as you want.

    ![text](https://raw.githubusercontent.com/mjhea0/sublime-setup-for-python/master/img/st3_package_control.png)

     
