# Linux Networking Fundamentals

## Target Audience

Networking and Data Communication is one of the fundamental skills of an SRE. This course is framed to cover the fundamentals of Linux Networking and practical examples of how these concepts are used by an SRE in day to day work. This course can be treated as quick refresher by already working SREs and can be skimmed faster

## Pre - Reads

This course requires high level knowledge of commonly used jargons in TCP/IP stack like  DNS, TCP, UDP and HTTP. Since this course is a part of the fundamental series, familiarity with jargons is sufficient to start this course. This course also expects basic exposure to Linux command line tools. There will be needs to install certain utilities and run them as part of course exercise.

## What to expect from this training

This course will try to cover what happens when somebody opens linkedin.com. Throughout the journey, the course covers how an SRE can optimize the system to improve her webstack performance and troubleshoot if there is an issue in any of the layers of networking stack. This course tries to dig through each layer of traditional TCP/IP stack and expects an SRE to have a picture beyond bird’s eye view of the functioning of the Internet

## What is not covered under this training

We are not covering concepts beyond fundamentals like HTTP/2.0, QUIC, TCP congestion control protocols, Anycast, BGP, CDN, Tunnels and Multicast. We expect post this course, one has relevant basics to have a quick grasp on such concepts

## Training Content

### Birds eye view of the course

The course covers the question “what happens when you open linkedin.com in your browser?” The course follows the flow of [TCP/IP stack](https://www.w3.org/People/Frystyk/thesis/TcpIp.html#TCPOSI).
The course spends time on two Application layer protocols DNS and HTTP, two transport layer protocols UDP and TCP, networking layer protocol IP and Data Link Layer protocol(generic)

### Lab Environment Setup

1. Dockerfile for this lecture is in ./build directory.
2. Build an image using the docker file and spawn two containers executing /bin/bash with the image.

```bash
sudo docker build -t network-demo .
#On one terminal use this to spawn an interactive bash container
sudo docker run -it network-demo /bin/bash
#On another terminal run the command to spawn another container running bash
sudo docker run -it network-demo /bin/bash
#Find the IP of each container using ip command
ip a
#Make sure both containers' IPs are reachable by ping from each other, Example
ping 172.17.0.3
```

![Illustration Video 1](https://user-images.githubusercontent.com/1917513/93758395-fdc8b480-fc25-11ea-9703-a0c25870d2b0.gif)

### Sections

<details>
<summary> <a href="https://github.com/kalyanceg/POC-Docs/blob/master/Linux-Networking/dns.md"> DNS </a> </summary>
<p>

- Domain Names
- Resolvers
- Dig
- Root domains & Authoritative NS
- Types of DNS records

</p>
</details>

<details>
<summary> <a href="https://github.com/kalyanceg/POC-Docs/blob/master/Linux-Networking/udp.md"> UDP </a> </summary>
<p>

- Multiplexing demultiplexing
- Example socket program

</p>
</details>

<details>
<summary> <a href="https://github.com/kalyanceg/POC-Docs/blob/master/Linux-Networking/http.md"> HTTP </a> </summary>
<p>

- Curl
- Request methods
- Response codes
- 1.0 vs 1.1
- Stateless and cookies
- Man in the middle- HTTPS

</p>
</details>

<details>
<summary> <a href="https://github.com/kalyanceg/POC-Docs/blob/master/Linux-Networking/tcp.md"> TCP </a> </summary>
<p>

- Sequence numbers
- 3 way handshake - retransmits
- Flow control
- Congestion control
- Socket close/tearing down connection

</p>
</details>

<details>
<summary> <a href="https://github.com/kalyanceg/POC-Docs/blob/master/Linux-Networking/ipr.md"> IP Routing and Data Link Layer </a> </summary>
<p>

- Routing table lookup
- ARP
- Link layer routing


</p>
</details>

