## Hardening
[[Linux Cheat Sheet]]
Several mechanisms are highly effective in securing Linux systems in keeping our and other companies' data safe. Three such mechanisms are SELinux, AppArmor, and TCP wrappers. These tools are designed to safeguard Linux systems against various security threats, from unauthorized access to malicious attacks, especially while conducting a penetration test. There is almost no worse scenario than when a company is compromised due to a penetration test. By implementing these security measures and ensuring that we set up corresponding protection against potential attackers, we can significantly reduce the risk of data leaks and ensure our systems remain secure. While these tools share some similarities, they also have important differences.

SELinux is a MAC system that is built into the Linux kernel. It is designed to provide fine-grained access control over system resources and applications. SELinux works by enforcing a policy that defines the access controls for each process and file on the system. It provides a higher level of security by limiting the damage that a compromised process can do.

AppArmor is also a MAC system that provides a similar level of control over system resources and applications, but it works slightly differently. AppArmor is implemented as a Linux Security Module (LSM) and uses application profiles to define the resources that an application can access. AppArmor is typically easier to use and configure than SELinux but may not provide the same level of fine-grained control.

TCP wrappers are a host-based network access control mechanism that can be used to restrict access to network services based on the IP address of the client system. It works by intercepting incoming network requests and comparing the IP address of the client system to the access control rules. These are useful for limiting access to network services from unauthorized systems.

Regarding similarities, the three security mechanisms share the common goal of ensuring the safety and security of Linux systems. In addition to providing extra protection, they can restrict access to resources and services, thus reducing the risk of unauthorized access and data breaches. It's also worth noting that these mechanisms are readily available as part of most Linux distributions, making them accessible to us to enhance their systems' security. Furthermore, these mechanisms can be easily customized and configured using standard tools and utilities, making them a convenient choice for Linux users.

In terms of differences, SELinux and AppArmor are both MAC systems that provide fine-grained access control over system resources but work in different ways. SELinux is built into the kernel and is more complex to configure and use, while AppArmor is implemented as a module and is typically easier to use. On the other hand, TCP wrappers are a host-based network access control mechanism designed to restrict access to network services based on the IP address of the client system. It is a simpler mechanism than SELinux and AppArmor but is useful for limiting access to network services from unauthorized systems.

---

## Setting Up
[[Linux Cheat Sheet]]
As we navigate the world of Linux, we inevitably encounter a wide range of technologies, applications, and services that we need to become familiar with. This is a crucial skill, particularly if we work in cybersecurity and strive to improve our expertise continuously. For this reason, we highly recommend dedicating time to learning about configuring important security measures such as `SELinux`, `AppArmor`, and `TCP wrappers` on your own. By taking on this (optional but highly efficient) challenge, you'll deepen your understanding of these technologies, build up your problem-solving skills, and gain valuable experience that will serve you well in the future. We highly recommend to use a personal VM and make snapshots before making changes.

When it comes to implementing cybersecurity measures, there is no one-size-fits-all approach. It is important to consider the specific information you want to protect and the tools you will use to do so. However, you can practice and implement several optional tasks with others in the Discord channel to increase your knowledge and skills in this area. By taking advantage of the helpfulness of others and sharing your own expertise, you can deepen your understanding of cybersecurity and help others do the same. Remember, explaining concepts to others is essential to teaching and learning.

#### SELinux

|||
|---|---|
|1.|Install SELinux on your VM.|
|2.|Configure SELinux to prevent a user from accessing a specific file.|
|3.|Configure SELinux to allow a single user to access a specific network service but deny access to all others.|
|4.|Configure SELinux to deny access to a specific user or group for a specific network service.|

#### AppArmor

|||
|---|---|
|5.|Configure AppArmor to prevent a user from accessing a specific file.|
|6.|Configure AppArmor to allow a single user to access a specific network service but deny access to all others.|
|7.|Configure AppArmor to deny access to a specific user or group for a specific network service.|

#### TCP Wrappers

|||
|---|---|
|8.|Configure TCP wrappers to allow access to a specific network service from a specific IP address.|
|9.|Configure TCP wrappers to deny access to a specific network service from a specific IP address.|
|10.|Configure TCP wrappers to allow access to a specific network service from a range of IP addresses.|

# Linux Security

---
[[Linux Cheat Sheet]]
All computer systems have an inherent risk of intrusion. Some present more of a risk than others, such as an internet-facing web server hosting multiple complex web applications. Linux systems are also less prone to viruses that affect Windows operating systems and do not present as large an attack surface as Active Directory domain-joined hosts. Regardless, it is essential to have certain fundamentals in place to secure any Linux system.

One of the Linux operating systems' most important security measures is keeping the OS and installed packages up to date. This can be achieved with a command such as:

Linux Security

```shell-session
xeloki@htb[/htb]$ apt update && apt dist-upgrade
```

If firewall rules are not appropriately set at the network level, we can use the Linux firewall and/or `iptables` to restrict traffic into/out of the host.

If SSH is open on the server, the configuration should be set up to disallow password login and disallow the root user from logging in via SSH. It is also important to avoid logging into and administering the system as the root user whenever possible and adequately managing access control. Users' access should be determined based on the principle of least privilege. For example, if a user needs to run a command as root, then that command should be specified in the `sudoers` configuration instead of giving them full sudo rights. Another common protection mechanism that can be used is `fail2ban`. This tool counts the number of failed login attempts, and if a user has reached the maximum number, the host that tried to connect will be handled as configured.

It is also important to periodically audit the system to ensure that issues do not exist that could facilitate privilege escalation, such as an out-of-date kernel, user permission issues, world-writable files, and misconfigured cron jobs, or misconfigured services. Many administrators forget about the possibility that some kernel versions have to be updated manually.

An option for further locking down Linux systems is `Security-Enhanced Linux` (`SELinux`) or `AppArmor`. This is a kernel security module that can be used for security access control policies. In SELinux, every process, file, directory, and system object is given a label. Policy rules are created to control access between these labeled processes and objects and are enforced by the kernel. This means that access can be set up to control which users and applications can access which resources. SELinux provides very granular access controls, such as specifying who can append to a file or move it.

Besides, there are different applications and services such as [Snort](https://www.snort.org/), [chkrootkit](http://www.chkrootkit.org/), [rkhunter](https://packages.debian.org/sid/rkhunter), [Lynis](https://cisofy.com/lynis/), and others that can contribute to Linux's security. In addition, some security settings should be made, such as:

- Removing or disabling all unnecessary services and software
- Removing all services that rely on unencrypted authentication mechanisms
- Ensure NTP is enabled and Syslog is running
- Ensure that each user has its own account
- Enforce the use of strong passwords
- Set up password aging and restrict the use of previous passwords
- Locking user accounts after login failures
- Disable all unwanted SUID/SGID binaries

This list is incomplete, as safety is not a product but a process. This means that specific steps must always be taken to protect the systems better, and it depends on the administrators how well they know their operating systems. The better the administrators are familiar with the system, and the more they are trained, the better and more secure their security precautions and security measures will be.

---

## TCP Wrappers

TCP wrapper is a security mechanism used in Linux systems that allows the system administrator to control which services are allowed access to the system. It works by restricting access to certain services based on the hostname or IP address of the user requesting access. When a client attempts to connect to a service the system will first consult the rules defined in the TCP wrappers configuration files to determine the IP address of the client. If the IP address matches the criteria specified in the configuration files, the system will then grant the client access to the service. However, if the criteria are not met, the connection will be denied, providing an additional layer of security for the service. TCP wrappers use the following configuration files:

- `/etc/hosts.allow`
    
- `/etc/hosts.deny`
    

In short, the `/etc/hosts.allow` file specifies which services and hosts are allowed access to the system, whereas the `/etc/hosts.deny` file specifies which services and hosts are not allowed access. These files can be configured by adding specific rules to the files.

#### /etc/hosts.allow

Linux Security

```shell-session
xeloki@htb[/htb]$ cat /etc/hosts.allow

# Allow access to SSH from the local network
sshd : 10.129.14.0/24

# Allow access to FTP from a specific host
ftpd : 10.129.14.10

# Allow access to Telnet from any host in the inlanefreight.local domain
telnetd : .inlanefreight.local

```

#### /etc/hosts.deny

Linux Security

```shell-session
xeloki@htb[/htb]$ cat /etc/hosts.deny

# Deny access to all services from any host in the inlanefreight.com domain
ALL : .inlanefreight.com

# Deny access to SSH from a specific host
sshd : 10.129.22.22

# Deny access to FTP from hosts with IP addresses in the range of 10.129.22.0 to 10.129.22.255
ftpd : 10.129.22.0/24
```

It is important to remember that the order of the rules in the files is important. The first rule that matches the requested service and host is the one that will be applied. It is also important to note that TCP wrappers are not a replacement for a firewall, as they are limited by the fact that they can only control access to services and not to ports.




# Firewall Setup

---

The primary goal of firewalls is to provide a security mechanism for controlling and monitoring network traffic between different network segments, such as internal and external networks or different network zones. Firewalls play a crucial role in protecting computer networks from unauthorized access, malicious traffic, and other security threats. Linux, being a popular operating system used in servers and other network devices, provides built-in firewall capabilities that can be used to control network traffic. In other words, they can filter incoming and outgoing traffic based on pre-defined rules, protocols, ports, and other criteria to prevent unauthorized access and mitigate security threats. The specific goal of a firewall implementation can vary depending on the specific needs of the organization, such as ensuring the confidentiality, integrity, and availability of network resources.

An example from the history of Linux firewalls is the development of the iptables tool, which replaced the earlier ipchains and ipfwadm tools. The iptables utility was first introduced in the Linux 2.4 kernel in 2000 and provided a flexible and efficient mechanism for filtering network traffic. iptables became the de facto standard firewall solution for Linux systems, and it has been widely adopted by many organizations and users.

The iptables utility provided a simple yet powerful command-line interface for configuring firewall rules, which could be used to filter traffic based on various criteria such as IP addresses, ports, protocols, and more. iptables was designed to be highly customizable and could be used to create complex firewall rulesets that could protect against various security threats such as denial-of-service (DoS) attacks, port scans, and network intrusion attempts.

In Linux, the firewall functionality is typically implemented using the Netfilter framework, which is an integral part of the kernel. Netfilter provides a set of hooks that can be used to intercept and modify network traffic as it passes through the system. The iptables utility is commonly used to configure the firewall rules on Linux systems.

---

## Iptables

The iptables utility provides a flexible set of rules for filtering network traffic based on various criteria such as source and destination IP addresses, port numbers, protocols, and more. There also exist other solutions like nftables, ufw, and firewalld. `Nftables` provides a more modern syntax and improved performance over iptables. However, the syntax of nftables rules is not compatible with iptables, so migration to nftables requires some effort. `UFW` stands for “Uncomplicated Firewall” and provides a simple and user-friendly interface for configuring firewall rules. UFW is built on top of the iptables framework like nftables and provides an easier way to manage firewall rules. Finally, FirewallD provides a dynamic and flexible firewall solution that can be used to manage complex firewall configurations, and it supports a rich set of rules for filtering network traffic and can be used to create custom firewall zones and services. It consists of several components that work together to provide a flexible and powerful firewall solution. The main components of iptables are:

|**Component**|**Description**|
|---|---|
|`Tables`|Tables are used to organize and categorize firewall rules.|
|`Chains`|Chains are used to group a set of firewall rules applied to a specific type of network traffic.|
|`Rules`|Rules define the criteria for filtering network traffic and the actions to take for packets that match the criteria.|
|`Matches`|Matches are used to match specific criteria for filtering network traffic, such as source or destination IP addresses, ports, protocols, and more.|
|`Targets`|Targets specify the action for packets that match a specific rule. For example, targets can be used to accept, drop, or reject packets or modify the packets in another way.|

#### Tables

When working with firewalls on Linux systems, it is important to understand how tables work in iptables. Tables in iptables are used to categorize and organize firewall rules based on the type of traffic that they are designed to handle. These tables are used to organize and categorize firewall rules. Each table is responsible for performing a specific set of tasks.

|**Table Name**|**Description**|**Built-in Chains**|
|---|---|---|
|`filter`|Used to filter network traffic based on IP addresses, ports, and protocols.|INPUT, OUTPUT, FORWARD|
|`nat`|Used to modify the source or destination IP addresses of network packets.|PREROUTING, POSTROUTING|
|`mangle`|Used to modify the header fields of network packets.|PREROUTING, OUTPUT, INPUT, FORWARD, POSTROUTING|

In addition to the built-in tables, iptables provides a fourth table called the raw table, which is used to configure special packet processing options. The raw table contains two built-in chains: PREROUTING and OUTPUT.

#### Chains

In iptables, chains organize rules that define how network traffic should be filtered or modified. There are two types of chains in iptables:

- Built-in chains
- User-defined chains

The built-in chains are pre-defined and automatically created when a table is created. Each table has a different set of built-in chains. For example, the filter table has three built-in chains:

- INPUT
- OUTPUT
- FORWARD

These chains are used to filter incoming and outgoing network traffic, as well as traffic that is being forwarded between different network interfaces. The nat table has two built-in chains:

- PREROUTING
- POSTROUTING

The PREROUTING chain is used to modify the destination IP address of incoming packets before the routing table processes them. The POSTROUTING chain is used to modify the source IP address of outgoing packets after the routing table has processed them. The mangle table has five built-in chains:

- PREROUTING
- OUTPUT
- INPUT
- FORWARD
- POSTROUTING

These chains are used to modify the header fields of incoming and outgoing packets and packets being processed by the corresponding chains.

`User-defined chains` can simplify rule management by grouping firewall rules based on specific criteria, such as source IP address, destination port, or protocol. They can be added to any of the three main tables. For example, if an organization has multiple web servers that all require similar firewall rules, the rules for each server could be grouped in a user-defined chain. Another example is when a user-defined chain could filter traffic destined for a specific port, such as port 80 (HTTP). The user could then add rules to this chain that specifically filter traffic destined for port 80.

#### Rules and Targets

Iptables rules are used to define the criteria for filtering network traffic and the actions to take for packets that match the criteria. Rules are added to chains using the `-A` option followed by the chain name, and they can be modified or deleted using various other options.

Each rule consists of a set of criteria or matches and a target specifying the action for packets that match the criteria. The criteria or matches match specific fields in the IP header, such as the source or destination IP address, protocol, source, destination port number, and more. The target specifies the action for packets that match the criteria. They specify the action to take for packets that match a specific rule. For example, targets can accept, drop, reject, or modify the packets. Some of the common targets used in iptables rules include the following:

|**Target Name**|**Description**|
|---|---|
|`ACCEPT`|Allows the packet to pass through the firewall and continue to its destination|
|`DROP`|Drops the packet, effectively blocking it from passing through the firewall|
|`REJECT`|Drops the packet and sends an error message back to the source address, notifying them that the packet was blocked|
|`LOG`|Logs the packet information to the system log|
|`SNAT`|Modifies the source IP address of the packet, typically used for Network Address Translation (NAT) to translate private IP addresses to public IP addresses|
|`DNAT`|Modifies the destination IP address of the packet, typically used for NAT to forward traffic from one IP address to another|
|`MASQUERADE`|Similar to SNAT but used when the source IP address is not fixed, such as in a dynamic IP address scenario|
|`REDIRECT`|Redirects packets to another port or IP address|
|`MARK`|Adds or modifies the Netfilter mark value of the packet, which can be used for advanced routing or other purposes|

Let us illustrate a rule and consider that we want to add a new entry to the INPUT chain that allows incoming TCP traffic on port 22 (SSH) to be accepted. The command for that would look like the following:

Firewall Setup

```shell-session
xeloki@htb[/htb]$ sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

#### Matches

`Matches` are used to specify the criteria that determine whether a firewall rule should be applied to a particular packet or connection. Matches are used to match specific characteristics of network traffic, such as the source or destination IP address, protocol, port number, and more.

|**Match Name**|**Description**|
|---|---|
|`-p` or `--protocol`|Specifies the protocol to match (e.g. tcp, udp, icmp)|
|`--dport`|Specifies the destination port to match|
|`--sport`|Specifies the source port to match|
|`-s` or `--source`|Specifies the source IP address to match|
|`-d` or `--destination`|Specifies the destination IP address to match|
|`-m state`|Matches the state of a connection (e.g. NEW, ESTABLISHED, RELATED)|
|`-m multiport`|Matches multiple ports or port ranges|
|`-m tcp`|Matches TCP packets and includes additional TCP-specific options|
|`-m udp`|Matches UDP packets and includes additional UDP-specific options|
|`-m string`|Matches packets that contain a specific string|
|`-m limit`|Matches packets at a specified rate limit|
|`-m conntrack`|Matches packets based on their connection tracking information|
|`-m mark`|Matches packets based on their Netfilter mark value|
|`-m mac`|Matches packets based on their MAC address|
|`-m iprange`|Matches packets based on a range of IP addresses|

In general, matches are specified using the '-m' option in iptables. For example, the following command adds a rule to the 'INPUT' chain in the 'filter' table that matches incoming TCP traffic on port 80:

Firewall Setup

```shell-session
xeloki@htb[/htb]$ sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
```

This example rule matches incoming TCP traffic (`-p tcp`) on port 80 (`--dport 80`) and jumps to the accept target (`-j ACCEPT`) if the match is successful.

|||
|---|---|
|1.|Launch a web server on TCP/8080 port on your target and use iptables to block incoming traffic on that port.|
|2.|Change iptables rules to allow incoming traffic on the TCP/8080 port.|
|3.|Block traffic from a specific IP address.|
|4.|Allow traffic from a specific IP address.|
|5.|Block traffic based on protocol.|
|6.|Allow traffic based on protocol.|
|7.|Create a new chain.|
|8.|Forward traffic to a specific chain.|
|9.|Delete a specific rule.|
|10.|List all existing rules.|