---
title: python中验证ip的有效性正则
tags:
  - python
date: 2015-03-12 19:57:30
---


```
def _chk_ipaddr(ipaddr):
    IP_PATTERN = '^((0|[1-9]\d?|[0-1]\d{2}|2[0-4]\d|25[0-5])\.){3}(0|[1-9]\d?|[0-1]\d{2}|2[0-4]\d|25[0-5])$'
    if not ipaddr:
        return False

    ipcheck = re.compile(IP_PATTERN, re.I)
    return True if ipcheck.match(ipaddr) else False
```

