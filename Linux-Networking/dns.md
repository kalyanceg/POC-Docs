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

![dns-ping](https://user-images.githubusercontent.com/1917513/93765030-dbd52f00-fc31-11ea-992e-d45c2423179d.gif)

If no match exists in /etc/hosts, the OS tries to do a DNS resolution using DNS protocol. The linux system makes a DNS request to the first IP in /etc/resolv.conf. If there is no response, requests will be sent to subsequent servers in resolv.conf. These servers in resolv.conf are called DNS resolvers. The DNS resolvers are populated by [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) when an IP address is assigned dynamically as the device connects to the network or statically configured by an administrator manually or using a config management system. 
[Dig](https://linux.die.net/man/1/dig) is a userspace DNS system which creates requests and sends to DNS resolvers and prints the response to the console.

```bash
#run this command in one container's bash to capture all DNS requests
sudo tcpdump -s 0 -A -i any port 53
#exec bash into the same container
sudo docker exec -it 1cfec84d0b3a /bin/bash
#make a dig request from the newly spawned bash
dig linkedin.com
```

![dns-tcpdump](https://user-images.githubusercontent.com/1917513/93766667-5ef78480-fc34-11ea-86c6-c0ee807829e5.gif)


The packet capture shows a request is made to 192.168.65.1:53 (this is the resolver in /etc/resolv.conf) for linkedin.com and a response is received from 192.168.65.1 with the IP address of linkedin.com 108.174.10.10

Now let's try to understand how DNS resolver tries to find the IP address of linkedin.com. DNS resolver first looks at its cache. Since many devices in the network can query for the domain name linkedin.com, there might be a hit in the cache. If it is a cache miss, it starts the DNS resolution process.
The DNS server breaks linkedin.com. to ., com., linkedin.com. and  starts DNS resolution from ‘.’. The . is called root domain and those IPs are known to the DNS resolver software. DNS resolver software asks root domain Nameservers who the right person would be to respond to details about ‘com.’. The authoritative nameserver of ‘com.’ will be given. Now the DNS resolution service will contact com’s authoritative nameserver to give the authoritative nameserver for linkedin.com. Once linkedin.com’s authoritative nameserver is known, the resolver asks Linkedin’s NS to provide IP address for linkedin.com. This whole process can be visualized by running dig +trace linkedin.com

```bash
dig +trace linkedin.com
```



