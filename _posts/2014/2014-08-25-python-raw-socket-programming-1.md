---
layout: post
title: 'Python - Raw Socket Programming #1'
tags:
  - python
  - socket
published: true
---


### IP Header (RFC 791) ###
```text
0                   1                   2                   3   
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|      Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |         Header Checksum       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source Address                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination Address                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```


### TCP Header (RFC 793) ###
```text
0                   1                   2                   3   
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |           |U|A|P|R|S|F|                               |
| Offset| Reserved  |R|C|S|S|Y|I|            Window             |
|       |           |G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### UDP Header (RFC 768) ###
```text
0      7 8     15 16    23 24    31  
+--------+--------+--------+--------+ 
|     Source      |   Destination   | 
|      Port       |      Port       | 
+--------+--------+--------+--------+ 
|                 |                 | 
|     Length      |    Checksum     | 
+--------+--------+--------+--------+ 
|                                     
|          data octets ...            
+---------------- ...                 
```


### Packet Sniffer Using Raw Socket ###
```python
# raw_socket.py

import socket, sys
from struct import *

IP_HDR_LEN  = 20
TCP_HDR_LEN = 20
PROTO = {"6" : "TCP", "17" : "UDP"}

def hexdump(src, length=8):
    result = []
    digits = 4 if isinstance(src, unicode) else 2
    for i in xrange(0, len(src), length):
       s = src[i:i+length]
       hexa = b' '.join(["%0*X" % (digits, ord(x))  for x in s])
       text = b''.join([x if 0x20 <= ord(x) < 0x7F else b'.'  for x in s])
       result.append( b"%04X   %-*s   %s" % (i, length*(digits + 1), hexa, text)  )
    return b'\n'.join(result)


if __name__ == "__main__":
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_TCP)
    except socket.error, msg:
        print 'Error Code : ' + str(msg[0]) + ' Msg ' + msg[1]
        sys.exit()

    while True:
        data, addr = s.recvfrom(65565)
        if addr[0] in ["127.0.0.1"]:
            continue
        print hexdump(data, 16)

        # IP Header
        ip_header = data[0:IP_HDR_LEN]
        iph = unpack('!BBHHHBBH4s4s', ip_header)

        version_ihl = iph[0]
        version = version_ihl >> 4
        ihl = version_ihl & 0xF
        iph_length = ihl * 4
        ttl = iph[5]
        protocol = iph[6]
        s_addr = socket.inet_ntoa(iph[8]);
        d_addr = socket.inet_ntoa(iph[9]);
        try:
            protocol_str=PROTO[str(protocol)]
        except:
            protocol_str="ETC"

        # TCP Header
        tcp_header = data[iph_length:iph_length+TCP_HDR_LEN]
        tcph = unpack('!HHLLBBHHH', tcp_header)

        s_port = tcph[0]
        d_port = tcph[1]
        sequence = tcph[2]
        acknowledgement = tcph[3]
        doff_reserved = tcph[4]
        tcphl = doff_reserved >> 4
        tcph_length = tcphl * 4

        # Data
        h_size = iph_length + tcph_length
        data_size = len(data) - h_size

        data = data[h_size:]

        print 'IPv' + str(version) + ', IPHdrLen: ' + str(iph_length) + ' TTL: ' + str(ttl) + ' Proto: ' + protocol_str + ", %s:%s -> %s:%s" % (str(s_addr), str(s_port), str(d_addr), str(d_port))
        print 'IPHdrLen: ' + str(iph_length) + ' TCPHdrLen: ' + str(tcph_length), 'Seq No: ' + str(sequence) + ' Ack: ' + str(acknowledgement)
        print 'Data : ' + data
```
