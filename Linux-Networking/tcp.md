#TCP

TCP is a transport layer protocol like UDP but it guarantees reliability, flow control and congestion control.
TCP guarantees reliable delivery by using sequence numbers. A TCP connection is established by a three way handshake. In our case, the client sends a SYN packet along with the starting sequence number it plans to use, the server acknowledges the SYN packet and sends a SYN with its sequence number. Once the client acknowledges the syn packet, the connection is established. Each data transferred from here on is considered delivered reliably once acknowledgement for that sequence is received by the concerned party

Thanks to wikipedia for this three way handshake illustration
![3-way handshake](https://upload.wikimedia.org/wikipedia/commons/f/f0/Three-way-handshake-example.gif)
