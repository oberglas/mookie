- [Cron](#Cron)
	- [Syntax](#Syntax)
		- [Format](#Format)
- [OSI Model](#OSI%20Model)
- [Ports](#Ports)
	- [Common ports](#Common%20ports)
	- [Docker networking](#Docker%20networking)
		- [Port mapping](#Port%20mapping)
	- [Firewalls and port blocking](#Firewalls%20and%20port%20blocking)
		- [Securing open ports](#Securing%20open%20ports)
	- [OSI Model and packet headers](#OSI%20Model%20and%20packet%20headers)
	- [Zero trust networking](#Zero%20trust%20networking)

## Cron

- Cron is used to schedule commands at a specific time, in the form of commands/tasks called "cron jobs".
- Cron is generally used for
	- Running scheduled backups
	- Monitoring disk space
	- Deleting files (i.e. log files) periodically
	- Running system maintenance tasks

### Syntax

- Display the contents of the contab file of the currently logged in user
	- `crontab -l`
- Edit the current user's cron jobs
	- `crontab -e`

#### Format

There are 6 parts to a cron command
1. Minute (0-59)
2. Hour (0-23)
3. Day of the month (1-31)
4. Month (1-12)
5. Day of the week (1-7)

The following cron job is scheduled to run every minute, every hour, every day of the month, every month, and every day of the week

	`* * * * * <command-to-execute>`

To run a cron job at every 5th minute, you would add the following to your crontab file
- `*/5 * * * * <command-to-execute>`
Every hour at minute 30
- `30 * * * * <command-to-execute>`
3 times every hour at minute 0, 5 and 10
- `0,5,10 * * * * <command-to-execute>`



# Networking

## OSI Model

The open systems interconnection model enables components of diverse systems to commnunicate using standard protocols

## Ports

- A port is a virtual point where network connections start and end.
- Each port is associated with a specific process and allow computers to easily differentiate between different kinds of traffic.
	- i.e. emails go to a differe2nt port than webpages

- Ports are standardized across all network-connected devices.
- Most ports are reserverd for certain protocols
	- All Hypertext Transfer Protocol messages => 80

- While IP addresses enable traffic to be sent to and from specific devices, port numbers allow targeting specific services within those devices.
- Ports help computers understand what to do with the many types of data they receive.

### Common ports

There are 65,535 possible port numbers -> https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml

### Docker networking

#### Port mapping

- Containers are conigured using parameters separated by a colon, indicating `<external>:<internal>` ports (from the perspective of the container)
	- For example, `-p 8080:80` would expose port 80 from inside the container to be accessible from the host's IP on port 8080 outside the container.
		- The internal port should generally stay the same, as each container has a different internal port. The external port should be unique, as the only 'external' port being referred to is the host computer's.

### Firewalls and port blocking

- A firewall is a system that blocks or allows network traffic based on a set of rules. They usually sit between a trusted network and an untrusted network (the internet, usually).
	- For example, an office network will use a firewall to protect their network from online threats.

- Some attackers try to send malicious traffic to random ports in the hopes that those ports have been left "open" - meaning they are allowed to receive traffic targeting that port.

#### Securing open ports

- The best thing is to not have anything running that's listening on a port. Limiting your attack surface by only having what you need running, and only having things listening on interfaces and ports you need is the best and first thing.
	- Other options include:
		- Configuring a firewall on the system the service is running on and blocking ports at the system level.
		- Use monitoring software on your network that
				A) Logs traffic for later analysis
				B) Updates common blacklists for IPs etc
				C) Looks for patterns in incoming traffic and sends alerts if anything unusual is found
		- Inserting a device between the router and switch that does any of the above
		- Restricting access through physical network topology or VLAN assignments
		- VPN/encrypted tunnel services, running on the edge of the network (on the router, or between the router and core switch) that only allows external access when it's authenticated and encrypted

### OSI Model and packet headers

- Ports are a Layer 4 (transport layer) concept. Only a transport protocol such as TCP or UDP can indicate which port a packet should go to.
	- TCP and UDP headers have a section for indicating port numbers.

- Network Layer protocols (IP for instance) have no way of knowing what port is in use for the given connection.
	- A standard IP header has no place to indicate which port the packet should go to, only the IP address - not the IP's port.

	- Ping software using Internet Control message Protocol (ICMP) packets, but do not have a way to specifiy the IP's port. 
	- Some ping software, such as My Traceroute, offers the option to send UDP headers on top of ICMP packets. This allows administrators to test specific ports within a networked device.

### Zero trust networking
- "The zero trust model treats all hosts as if they're internet-facing, and considers the entire network to be compromised and hostile. By taking this approach, you'll focus on building strong authentication, authorization, and encryption throughout, whil eproviding compartmentalized access and better operational agility." https://www.amazon.com/dp/B072WD347M/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1

