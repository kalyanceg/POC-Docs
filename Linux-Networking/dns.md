# DNS
Domain Names are simpler human readable names for websites. The Internet understands only IP addresses, but since memorizing incoherent numbers is difficult simple domain names are used instead. These domain names are translated into IP addresses by the DNS infrastructure. When somebody tries to open linkedin.com in the browser, the browser tries to convert linkedin.com to IP Address. This process is called DNS resolution. A simple pseudocode looks this

```python
ip, err = getIPAddress(domainName)
if err:
  print(“unknown Host Exception while trying to resolve:%s”.format(domainName))
```

Now let’s try to understand what happens inside the getIPAddress function. The browser would have a dns cache of its own where it checks if there is a mapping for the domainName if there is a matching available, then the browser uses that IP address. If there is no mapping, the browser calls gethostbyname syscall to ask the Operating system to find the IP address for a given DomainName

```python
def getIPAddress(domainName):
    resp, fail = lookupCache(domainName)
    If not fail:
       return resp
    else:
       resp, err = gethostbyname(domainName)
       if err:
         return null, err
       else:
          return resp
```

Now lets understand what Operating system kernel does when the gethostbyname function is called. The Linux operating system looks at the file [/etc/nsswitch.conf](https://man7.org/linux/man-pages/man5/nsswitch.conf.5.html) file which usually has a line

```bash
hosts:      files dns
```

This line means the host lookup has to look up first in file (/etc/hosts) and then use DNS protocol to do the resolution if there is no match in /etc/hosts. The file /etc/hosts is of format
IPAddress FQDN[,FQDN]

```bash
127.0.0.1 localhost.localdomain localhost
::1 localhost.localdomain localhost
```

If a match exists for a domain in this file then that IP address is returned by the OS. Lets add a line to this file

```bash
127.0.0.1 test.linkedin.com
```

And then do ping test.linkedin.com

```bash
ping test.linkedin.com -n
```
