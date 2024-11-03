# Network-Scanning-Attack

## Network Device Discovery

To identify devices on the local network, I ran the following command:

```bash
arp-scan <ip/subnet>
```

This command sends ARP requests to all IPs within the specified subnet. Any device that responds with an ARP reply reveals its IP and MAC address, indicating it currently occupies that IP.


![Alt text](https://cdn-images-1.medium.com/max/1200/1*pccFhfYIzUVjH1cwgU-gGA.png)


## Port Scanning

After identifying IP addresses of devices on the local network, I scanned for open ports on each host using:

```bash
nmap -sT <IP>
```

![Alt text](https://cdn-images-1.medium.com/max/1200/1*iJdDy5Wla6iyDL38Rt8LKg.png)


The -sT flag initiates a TCP scan, sending SYN packets to each port and receiving ACK packets from the open ones. Additionally, the -O flag can be used to attempt OS detection The pipe and tail at the end of the command was used so we only received the last 10 lines of the output. :

```bash
nmap -O <IP> | tail 10

```
![Alt text](https://cdn-images-1.medium.com/max/1200/1*xHT-sC9g5-NpZSt9RA8wcQ.png)

## Zenmap
A tool that allowed for a user-friendly GUI for network scanning, allowing for visual investigation of scan results.

![Alt text](https://cdn-images-1.medium.com/max/1200/1*V8gIUNWsfEi9ag83Y2598Q.png)


# Exploitation

## Metasploit Setup
After scanning the network, I switched to the Metasploit framework for exploitation. I performed a database scan with:

```bash
db_nmap <xxx.xxx.xxx.*>
```

![Alt text](https://cdn-images-1.medium.com/max/1200/1*IuJZFqXimELYth84nRnyhQ.png)

![Alt text](https://cdn-images-1.medium.com/max/1200/1*mNpES1ARhrTarr7t7CKJjA.png)


## Exploiting Vulnerabilities
Using Armitage, I ran an attack scan to identify potential vulnerabilities. One of the first attacks was java_rmi_server, which exploits the Java RMI registry, used for Java objects to communicate across networks. Due to improper mitigations, I was able to gain unauthorized access to the system.

![Alt text](https://cdn-images-1.medium.com/max/1200/1*PekJRMk1q9wRfWZ78DC9ew.png)

Through this access, I obtained a command shell on the target system. I accessed the shadow file, containing hashed administrative credentials. Using John the Ripper, I successfully cracked the hash and retrieved the admin password.

![Alt text](https://cdn-images-1.medium.com/max/1200/1*RmDHejhT7M154ior4albCg.png)

![Alt text](https://cdn-images-1.medium.com/max/1200/1*aeh2SMnNKzyCDLIHYmPXQw.png)


Privilege Escalation and Lateral Movement
With the cracked admin credentials, I accessed another system on the network via Telnet, further compromising the network.

![Alt text](https://cdn-images-1.medium.com/max/1200/1*oJWtBC5bUia-r9qg_lkypw.png)


## Firewall Compromise
Finally, I noticed the firewall (PfSense) was configured with default credentials. After finding these credentials online, I logged in via SSH and gained access to the firewall, compromising it as well.

![Alt text](https://cdn-images-1.medium.com/max/1200/1*wpjf4NRp-rON4hKwGJ09nA.png)


![Alt text](https://cdn-images-1.medium.com/max/1200/1*RDfJLd4QSHo8yOT_oZJByw.png)

