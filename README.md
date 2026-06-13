# NetPractice
This project focuses on understanding the fundamentals of computer networking, IP addressing, and subnetting through practical configuration exercises.

[Click here if you want to go straight to the subnetting cheat sheet](#subnetting-cheat-sheet-video)

![127.0.0.1 joke](https://giftstory.pl/userdata/public/gfx/1238/There-is-no-place.jpg)

## Core Concepts
- **IP Addressing**: Structure and classification of IPv4 addresses.
- **Subnet Masking**: Division of networks into smaller segments.
- **Routing Tables**: Configuration of rules for data packet transmission.
- **Troubleshooting**: Analysis and resolution of network configuration errors.

## Skills Acquired
- Calculation of network and broadcast addresses.
- Determination of valid host address ranges.
- Configuration of default gateways.
- Diagnostics of network connectivity issues.

## Why is it still not OK even though the math is correct? i.e. non-routing addresses
Somewhere around level 9, I have fallen into a trap. Math was correct, and it still wasn't working. It was because of the simple fact that I was using a non-routable IP address.

```
A programmer gets into a taxi.
– Taxi driver: "Where to, mate?"
– Programmer: "192.168.0.13"
– Taxi driver: "No way, buddy. I can't take you there. That's a private address!"
```

Non-routable IP addresses are reserved for private networks and are not globally unique. This means that they cannot be used to communicate directly over the Internet. Instead, they are used within a local network, such as a home or business network, to allow devices to communicate with each other.


## Subnetting Cheat Sheet [(Video)](https://www.youtube.com/watch?v=ljS07YTEJ2I&list=PLIFyRwBY_4bQUE4IB5c4VPRyDoLgOdExE&index=2&pp=iAQB)

| Group size        | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|------------------|-----|----|----|----|---|---|---|---|
| Subnet mask      | 128 | 192 | 224 | 240 | 248 | 252 | 254 | 255 |
| CIDR (4th octet) | /25 | /26 | /27 | /28 | /29 | /30 | /31 | /32 |
| CIDR (3rd octet) | /17 | /18 | /19 | /20 | /21 | /22 | /23 | /24 |
| CIDR (2nd octet) | /9  | /10 | /11 | /12 | /13 | /14 | /15 | /16 |
| CIDR (1st octet) | /1  | /2  | /3  | /4  | /5  | /6  | /7  | /8 |

## Resources
- [ibm TCP/IP addressing](https://www.ibm.com/docs/en/aix/7.3.0?topic=protocol-tcpip-addressing)
- [what is TCP/IP and OSI](https://www.youtube.com/watch?v=CRdL1PcherM)