02-10-202514:33
Status:
Tags: [networking](networking.md)

# DHCP

## Attack Vectors:
DHCP Spoofing
	an attacker sets up a rogue DHCP server with the network and provides malicious network configurations, such as:
		1) A fake default gateway - man in the middle
		2) malicious DNS server - leads to alternate sites
	Consequences:
		Data interception modification or injection
	Mitigation Strategies
		Enable [DHCP Snooping](DHCP%20Snooping.md) on network switches to filter untrusted DHCP servers.
		Implement **IP Source Guard** in combination with DHCP Snooping to prevent IP-MAC spoofing
		use **802.1x authentication** to verify the legitimacy of network devices
DHCP Starvation Attack
	Issue:
		An attacker floods the network with numerous fake DHCP requests, consuming all available IP addresses.
	Consequences:
		Causes [DoS](denial%20of%20service.md) for legitimate users who can no longer obtain an IP address.
		Can be a precursor to **DHCP Spoofing**, allowing an attacker's rogue DHCP server to take control.
	Mitigation Strategies:
		Implement **Port Security** to limit the number of MAC addresses per switch port.
		Use [DHCP Snooping](DHCP%20Snooping.md) to track and block excessive DHCP requests from unauthorized sources.
Man in the Middle Attack
	Issue:
		Attackers manipulate DHCP configurations to redirect client traffic through their own system.
	Consequences:
		Data interception, modification, or eavesdropping
Rogue Client Attack
	Issue:
		A malicious client attempts to use an unauthorized IP address or MAC address to evade security policies
	Consequences:
		IP conflicts
		Privilege escalation
	Mitigations Strategies:
		Use **DHCP Binding Table** with ARP Inspection to validate IP-MAC bindings
		VLAN isolation
		Access Control Lists to limit unauthorized communications
## Privacy Issues:
Since DHCP exchanges **device-specific identifiers**, they can expose user info and facilitate tracking

Device Identifier Leakage
	Issue:
		DHCP requests include sensitive details such as:
			MAC addresses
			Hostname and OS information
	Consequences
		User tracking
		fingerprinting
	Mitigation Strategies
		enable randomized MAC adresses
		Configure anonymous DHCP requests
		use private [VLAN](VLAN)s to limit exposure
DCHP Lease Time and Log Tracking
	Issue:
		DHCP servers store logs containing
			IP address allocations
			Lease time records which can be use d for behavioral analytics.
	Consequences:
		Surveillance by corporations or ISPs to monitor user activity.
		Data Leaks if logs are not properly secured.
	Mitigation Strategies:
		Periodically delete old DHCP logs to minimize exposure
		Use log encryption to protect sensitive information
		Implement data minimization policies to reduce unnecessary tracking.
## Trust Issues:
Since DHCP is a trust-based, unauthenticated protocol, it relies on implicit assumptions about the validity of its messages

Trusting the DHCP server
	Issue:
		clients automatically accept DHCP configurations without verifying the legitimacy of the server.
	Consequences:
		Misconfigured or malicious DHCP servers can disrupt network functionality
		Attackers can hijack traffic by providing false gateways or DNS servers
	Mitigation Strategies:
		Define a trusted DHCP server list
		use Secure DHCP to validate server identity
		Deploy Network Access Control systems to verify endpoint security compliance
Trust between DHCP servers
	Issue:
		In networks with multiple DHCP servers, inconsistency or untrusted servers can cause conflicts
	Consequences:
		IP allocation conflicts
		Network instability due to overlapping address pools
	Mitigation Strategies:
		Implement DHCP Failover Protocols to sync lease data
		Use DHCP Relay Agents to ensure responses come only from trusted servers
Trusting the DHCP client
	Issue:
		The DHCP client does not validate the authenticity of the assigned IP, gateway, or DNS setigns.
	Consequences:
		Exposure to DNS hijacking, leading to phishing and malware attacks.
		False routing configurations, causing users to connect to attacker-controlled networks.
	Mitigation Strategies:
		Configure static IP settings for critical systems.
		Enforce [DNSSEC](DNSSEC) to validate DNS responses and prevent tampering.
# References

