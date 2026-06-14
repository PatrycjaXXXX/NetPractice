*This project has been created as part of the 42 curriculum by psmolich.*

# NetPractice
### Description
This project focuses on understanding the fundamentals of computer networking, IP addressing, and subnetting through practical configuration exercises.

#### Core Concepts
- **IP Addressing**: Structure and classification of IPv4 addresses.
- **Subnet Masking**: Division of networks into smaller segments.
- **Routing Tables**: Configuration of rules for data packet transmission.
- **Troubleshooting**: Analysis and resolution of network configuration errors.

#### Skills Acquired
- Calculation of network and broadcast addresses.
- Determination of valid host address ranges.
- Configuration of default gateways.
- Diagnostics of network connectivity issues.

Click here if you want to go straight to the [subnetting cheat sheet](#subnetting-cheat-sheet-video) or [special-purpose addresses](#ipv4-special-purpose-addresses)

### Instructions
First, download the file attached to the project’s page. Then, extract the files into any folder of your choice.
Extracting files:
```
tar -xf net_practice.1.9.tgz
```
Running the training interface:
```
cd net_practice.1.9
chmod +x run.sh
./run.sh
```
This interface should open in your web browser:

![training interface](img/Screenshot%20from%202026-06-14%2013-15-49.png)

Now you can practice by entering your login in the field to use your personal configuration. Alternatively, you can use the "evaluation" tab to generate a random configuration,
also suitable for evaluations.

Submission: Because there are 10 levels available in the training interface, you will have to submit 10
files in your repository (one file per level). Place them at the root of your repository.
Don’t forget to enter your login in the training interface. Export a file per level using the
Get my config button.

Side note: if for some reason you are not able to use the training interface from provided .tgz file, you can use the [web version](https://iaceene.github.io/Net_Practice_42/light/) created by [Yassine Ajagrou](https://github.com/iaceene).

## Devices
- **Host**: a device that can act as a source or destination of network traffic.
- **Switch**: a Layer 2 device that connects hosts within the same local network and subnet. Initially, it builds its MAC address table by learning the source address of any incoming data frame and mapping it to the receiving port. Once the table is populated, the switch identifies devices by looking up the destination MAC address of subsequent frames. This allows it to forward data directly to the correct port instead of flooding the entire network. Because it operates at the data-link layer, all connected devices must share the same subnet to communicate without a router.
```
Devices connected to a switch can only communicate directly if they are configured to be in the same subnet; otherwise, traffic is sent to a router.
If both computers are physically connected to the same switch but belong to different subnets and there is no router between them, they will still not communicate because the TCP/IP stack will assume the destination is located outside the local network and will look for a gateway to forward the traffic. If no gateway is configured, the communication will fail.
```
- **Router**: a Layer 3 device that connects different networks and subnets. It uses routing tables to determine the best path for forwarding packets between networks. Routers enable communication between hosts in different subnets and are also responsible for functions such as Network Address Translation (NAT), allowing private IP addresses to access public networks.
```
You need router to cross subnet boundries.
```

## Local or should be routed? How is it decided?
When a host wants to communicate with another device, it first checks if the destination IP address is within the same subnet. This is determined by performing a bitwise AND operation between the destination IP address and the subnet mask of the host. If the result matches the network portion of the host's own IP address, then the destination is considered local, and the host will attempt to communicate directly with it. If the destination is not local, the host will send the traffic to its default gateway (usually a router) for routing to the appropriate network.

CIDR defines how many leading bits of an IP address belong to the network portion. The host compares its own network prefix with the destination’s network prefix. If both prefixes *match*, the destination is **local**. If they *differ*, traffic is sent to the **default gateway**.

*Subnet mask definition: number that "masks" an IP address, dividing it into a routing prefix and a host address*

Example for a host with IP address `192.168.1.10` and subnet mask `255.255.255.248` (or `/29` in CIDR notation, *8 bit groups size*):
```
Host IP:        192.168.1.10      → 11000000.10101000.00000001.00001010
Subnet mask:    255.255.255.248   → 11111111.11111111.11111111.11111000 (29 leading bits for network, 3 bits for hosts)

Bitwise AND:
11000000.10101000.00000001.00001010
11111111.11111111.11111111.11111000
=
11000000.10101000.00000001.00001000  → 192.168.1.8/29 (network)
```
Only hosts subnet is used to check, as it defines group size:
```
Destination:    192.168.1.14      → 11000000.10101000.00000001.00001110

Bitwise AND:
11000000.10101000.00000001.00001110
11111111.11111111.11111111.11111000
=
11000000.10101000.00000001.00001000  → 192.168.1.8

=> SAME NETWORK → LOCAL (ARP used)
```
```
Destination:    192.168.1.22      → 11000000.10101000.00000001.00010110

Bitwise AND:
11000000.10101000.00000001.00010110
11111111.11111111.11111111.11111000
=
11000000.10101000.00000001.00010000  → 192.168.1.16

=> DIFFERENT NETWORK → ROUTE VIA DEFAULT GATEWAY
```

![127.0.0.1 joke](https://giftstory.pl/userdata/public/gfx/1238/There-is-no-place.jpg)

## Why is it still not OK even though the math is correct? i.e. non-routing addresses
Somewhere around level 9, I have fallen into a trap. Math was correct, and it still wasn't working. It was because of the simple fact that I was using a non-routable IP address.

```
A programmer gets into a taxi.
– Taxi driver: "Where to, mate?"
– Programmer: "192.168.0.13"
– Taxi driver: "No way, buddy. I can't take you there. That's a private address!"
```

*Non-routable IP addresses are reserved for private networks and are not globally unique. This means that they cannot be used to communicate directly over the Internet. Instead, they are used within a local network, such as a home or business network, to allow devices to communicate with each other.*

Even though your computer may have an IP address like `192.168.242.42`, that address is only meaningful within your local network. It is not directly reachable from the public Internet.

This works thanks to **Network Address Translation**, a mechanism commonly implemented on home and enterprise routers. **NAT** allows multiple devices with private IP addresses to share a single public IP address when communicating with the Internet.

In Level 9, we will use a form of NAT known as **Static NAT (SNAT)**, where a specific private address is permanently mapped to a specific public address. This makes selected hosts reachable from outside the local network.

The following address ranges are defined by RFC 1918 and are not routed on the public Internet:

* single class A network `10.0.0.0/8`
  Range: `10.0.0.0 – 10.255.255.255`

* 16 contiguous class B networks `172.16.0.0/12`
  Range: `172.16.0.0 – 172.31.255.255`

* 256 contiguous class C networks`192.168.0.0/16`
  Range: `192.168.0.0 – 192.168.255.255`

Routers on the Internet will not forward packets originating from or destined for these ranges. To communicate with public networks, private addresses must be translated into publicly routable addresses using NAT.

## IPv4 Special-Purpose Addresses

| CIDR | Purpose | Why we don't use it as a normal public address |
|--------|---------|-----------------------------------------------|
| `0.0.0.0/8` | Unspecified addresses | Used to represent "no address" or "any address". |
| `127.0.0.0/8` | Loopback | Traffic never leaves the host; used for localhost communication. |
| `10.0.0.0/8` | Private network | Reserved by RFC1918 and not routed on the Internet. |
| `100.64.0.0/10` | Carrier-Grade NAT | Reserved for ISP NAT infrastructure. |
| `169.254.0.0/16` | Link-local | Automatically assigned when DHCP is unavailable. |
| `172.16.0.0/12` | Private network | Reserved by RFC1918 and not routed on the Internet. |
| `192.168.0.0/16` | Private network | Reserved by RFC1918 and not routed on the Internet. |
| `224.0.0.0/4` | Multicast | Used to send traffic to groups of hosts. |
| `240.0.0.0/4` | Reserved | Reserved for future use and not generally routable. |
| `255.255.255.255/32` | Broadcast | Sends traffic to all hosts on the local network. |

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
- [Non-Routable IP Addresses](https://ipstack.com/non-routable-ip-address/)
- [Private Networks](https://en.wikipedia.org/wiki/Private_network)
- [Network Address Translation (NAT)](https://www.youtube.com/watch?v=wg8Hosr20yw)
- [Link-Local Addresses](https://en.wikipedia.org/wiki/Link-local_address#IPv4)
- [Default Gateway](https://www.youtube.com/watch?v=pCcJFdYNamc)
- [Hub, Switch, Router](https://www.youtube.com/watch?v=1z0ULvg_pW8)
- [Routing Tables](https://www.youtube.com/watch?v=CGmTvukObOw)
- [Subnet mask](https://www.wikidata.org/wiki/Q1245638)
- [Yassine Ajagrou](https://github.com/iaceene)

## How AI was used
- [LanguageTool](https://languagetool.org/) is a tool powered by artificial intelligence (AI) and natural language processing (NLP) technology. It doesn't generate entire texts from scratch, it uses AI to check grammar, spelling, and style.
- [Gemini Chatbot](https://gemini.google.com/) was used to check if the information provided by me was correct, and it also generously offered me the taxi joke. For example, the table of special-purpose addresses got reviewed. As it's almost an integral part of Google, it also provided me with webpages such as [ipstack](https://ipstack.com).
- The formatting and structuring of this README were assisted by **GitHub Copilot**.