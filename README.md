# linux-UDP
## UDP socket communication in linux by creating Padawan Connection
### Introduction
This is a reliable transport protocol over the UDP protocol called `Padawan Transport Protocol (PTP)`.<br>
PTP will include most (but not all) of the TCP features.<br>
Examples of these features include timeout, ACK, sequence numbers, etc. <br>
### About Sender
This section provides details on the Sender. <br>
The Sender accept the following eight arguments. <br>
* receiver_host_ip: the IP address of the host machine on which the Receiver is running.<br>
* receiver_port: the port number on which Receiver is expecting to receive packets from
the sender. This should match the command line argument of the same name for the Receiver.
* FileToSend.txt: the name of the text file that has to be transferred from sender to
receiver using your reliable transport protocol. You may assume that the file included in the
argument will be available in the current working directory of the Sender with the correct
access permissions set (read).
* MWS: the maximum window size used by your PTP protocol in bytes.
* MSS: Maximum Segment Size which is the maximum amount of data (in bytes) carried in
each PTP segment. NOTE: In our tests we will ensure that MWS is exactly divisible by MSS.
* timeout: the value of timeout in milliseconds.
* pdrop: the probability that a PTP data segment which is ready to be transmitted will be
dropped. This value must be between 0 and 1. For example if pdrop = 0.5, it means that
50% of the transmitted packets are dropped by the PL.
* seed: The seed for your random number generator. 
### PL module
The function of the PL is to
emulate packet loss on the Internet. Even though theoretically UDP datagrams will get lost, in our
test environment these events will occur very rarely.<br>

The following describes the sequence of steps that the PL should perform on receiving a PTP
segment:
* If the PTP segment is for connection establishment or teardown, then pass the segment to
UDP, do not drop it.
* If the PTP segment is not for connection establishment or teardown, the PL module must do
one of the following:<br>
(a) with probability pdrop drop the datagram.<br>
(b) With probability (1-pdrop), forward the datagram.<br>

### About Receiver
The Receiver accept the following two arguments: <br>
* receiver_port: the port number on which the Receiver will open a UDP socket for
receiving datagrams from the Sender.<br>
* FileReceived.txt: the name of the text file into which the text sent by the sender
should be stored (this is the file that is being transferred from sender to receiver).<br>

### File List
* server.c
* receiver.c
* makefile
* FileToSend.txt
* FileReceived.txt
## Usage
First run the receiver. <br>
```bash
./receiver receiver_port FileReceived.txt
```
Then run the sender. <br>
```bash
./sender receiver_host_ip receiver_port FileToSend.txt MWS MSS timeout pdrop seed
```


