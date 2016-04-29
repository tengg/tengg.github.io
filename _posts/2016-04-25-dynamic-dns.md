---
layout: post
title: Update Dynamic DNS via Crontab Script
---

Ok, we have registered a domain name, and we have purchased a VPS to host our server. But why not assigning a subdomain to the computer at home?


### What's dynamic DNS?

Internet providers change the home IP address from to time to time, therefore it won't work if we set an A Record for the home computer. Fortunately most DNS manager on your registrar support dynamic DNS record, which can be updated easily by the software they provide or by their API. Apparently the APIs are preferable to me.

### Update dynamic DNS record by Namecheap.com's API

I recently registered this domain name on namecheap.com, who provides a fairly easy API to update the dynamic DNS.
<!--more-->

> How do I use the browser to dynamically update host's IP?
>
> Please substitute appropriate values for host, domain, password and IP:
>
> https://dynamicdns.park-your-domain.com/update?host=**[host]**&domain=**[domain_name]**&password=**[ddns_password]**&ip=**[your_ip]**
>
> If you want to update an IP address for  domain (e.g. home.yourdomain.tld), then you should specify the following details:
>
> **Host** = home
>
> **Domain Name**= yourdomain.tld
>
> **Dynamic DNS Password** = Domain List >> click Manage next to the domain >>Advanced DNS tab >> Dynamic DNS. If it is not enabled, enable it to check the password.
>
> **IP Address**= an optional value. If you don't specify any IP, the IP from which you are accessing this URL will be set for the domain.
>
> NOTE: The values for host name and domain must be in lower case. Please make sure your are using your Dynamic DNS password but not the Namecheap account's one.
>
> In this case the URL will have the following format:
>
> https://dynamicdns.park-your-domain.com/update?host=home&domain=yourdomain.tld&password=e747d77054a844409c486973cb&ip=127.0.0.1
>

Based on these information, we can write a script to update the dynamic DNS entry, and put it into crontab for a scheduled execution.

```python
#!/usr/bin/env python
#This is ddns.py

import requests
import time

def ddns(ip):
    conn = requests.get("https://dynamicdns.park-your-domain.com/update?host=home&domain=[YOUR_DOMAIN]&password=[PASSWORD_FROM_NAMECHEAP]&ip=%s" % ip)

def getip():
    return requests.get('https://domains.google.com/checkip').text

if __name__ == '__main__':
    print(time.ctime())
    try:
        ip = getip()
        print(ip)
        ddns(ip)
    except Exception, e:
        pass
```

Next, we edit the cron jobs by:

```sh
$ crontab -e
```

Adding following line to enable the cronjob run every 15 minutes and record the stdout:

```
*/15 * * * * /usr/local/bin/python [path]/ddns.py > [path]/ddns.log 2>[path]/ddns.log
```

