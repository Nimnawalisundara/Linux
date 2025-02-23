# Assignment 8 #

## Firewall Configuration - Linux ##

- Configuring a firewall in Linux is essential for securing your server against unauthorized access and various types of attacks.

### Base configuration ###

1. Enable UFW on system start
    - sudo systemctl enable ufw
    - sudo ufw enable

    ![](/img/Assignment8/1.PNG)

2. Default Policies
    - sudo ufw default deny incoming
    - sudo ufw default allow outgoing


    ![](/img/Assignment8/2.PNG)

### Service Rules ###

1. SSH Server

    - sudo ufw allow 22/tcp
        - To allow traffic on port 22 for SSH.
    - sudo ufw limit ssh comment 'Rate limit SSH connections'.
        - To rate limit SSH connections is a great way to protect your server from brute force attacks.

        ![](/img/Assignment8/3.PNG)

2. Web Server (HTTP/HTTPS) rules

    - sudo ufw allow 80/tcp
        - Allow incoming traffic on port 80, which is typically used for HTTP.
    - sudo ufw allow 443/tcp
        - Allow incoming HTTPS traffic on port 443.

    ![](img/Assignment8/4.PNG)

3. Logging Configuration

    - sudo ufw logging on
        - This will help to monitor all connections and identify any potential security issues.
    - sudo ufw logging high
        - Provide more detailed logs for network traffic, which can be very useful for troubleshooting and monitoring purposes.

    ![](/img/Assignment8/5.PNG)

#### Protection Against Common Attacks ####

##### SYN Flood Protection #####

1. Open the /etc/sysctl.conf file with nano:
    - sudo nano /etc/sysctl.conf

2. Scroll to the bottom of the file or find an appropriate place to add the new setting.

3. Add the following line to enable TCP SYN cookies:
    - net.ipv4.tcp_syncookies = 1
    - net.ipv4.tcp_max_syn_backlog = 2048
        - Increase backlog queue to handle more concurrent SYN packets
    - net.ipv4.tcp_synack_retries = 2
        - Reduce number of SYN-ACK retries to limit resource consumption
    - net.ipv4.tcp_syn_retries = 5
    - net.ipv4.tcp_max_syn_backlog = 2048
    - net.ipv4.tcp_syncookies = 1
    - net.ipv4.tcp_ecn = 0
    - net.ipv4.tcp_wmem = 4096 87380 8388608
    - net.ipv4.tcp_rmem = 4096 87380 8388608
        
    ![](/img/Assignment8/6.PNG)

4. Apply settings
    - sudo sysctl -p

    ![](/img/Assignment8/7.PNG)

##### Additional Protection Measures #####

Block invalid packets
***What are invalid packets?***

- Invalid packets include any incoming connections that don’t start with the SYN flag alone.
- In a standard TCP Three-Way Handshake, every new connection begins with a SYN packet.

***How UFW blocks invalid packets***

Default rules added by UFW on /etc/ufw/before.rules file

1. drop INVALID packets (logs these in loglevel medium and higher)
    - A ufw-before-input -m conntrack --ctstate INVALID -j ufw-logging-deny
    - A ufw-before-input -m conntrack --ctstate INVALID -j DROP

2. Add these two rules below the ones added by default by UFW:

    - -A ufw-before-input -p tcp -m tcp ! --tcp-flags FIN,SYN,RST,ACK SYN -m conntrack --ctstate NEW -j ufw-logging-deny
    - -A ufw-before-input -p tcp -m tcp ! --tcp-flags FIN,SYN,RST,ACK SYN -m conntrack --ctstate NEW -j DROP

    ![](/img/Assignment8/8.PNG)

3. Don't forget to add them to the before6.rules file as well:

    - -A ufw6-before-input -p tcp -m tcp ! --tcp-flags FIN,SYN,RST,ACK SYN -m conntrack --ctstate NEW -j ufw6-logging-deny
    - -A ufw6-before-input -p tcp -m tcp ! --tcp-flags FIN,SYN,RST,ACK SYN -m conntrack --ctstate NEW -j DROP

#### Block ping (ICMP) requests ####
*** What is ICMP flood attack?***

- An ICMP flood attack, also known as a ping flood, is a type of Denial of Service (DoS) attack. It overwhelms a system by sending a huge number of ICMP packets, specifically echo requests (pings).

***How to prevent ICMP requests***

- By commenting the lines below to block ping requests (icmp protocol) by ufw on file /etc/ufw/before.rules:

1. ok icmp codes for INPUT
    1. -A ufw-before-input -p icmp --icmp-type destination-unreachable -j ACCEPT
    2. -A ufw-before-input -p icmp --icmp-type time-exceeded -j ACCEPT
    3. -A ufw-before-input -p icmp --icmp-type parameter-problem -j ACCEPT
    4. -A ufw-before-input -p icmp --icmp-type echo-request -j ACCEPT

        ![](/img/Assignment8/9.PNG)

2. ok icmp code for FORWARD
    1. -A ufw-before-forward -p icmp --icmp-type destination-unreachable -j ACCEPT
    2. -A ufw-before-forward -p icmp --icmp-type time-exceeded -j ACCEPT
    3. -A ufw-before-forward -p icmp --icmp-type parameter-problem -j ACCEPT
    4. -A ufw-before-forward -p icmp --icmp-type echo-request -j ACCEPT

    ![](img/Assignment8/10.PNG)

2. Now, reload UFW
    - sudo ufw reload

    ![](/img/Assignment8/11.PNG)

3. Verification and Monitoring
    - Display current rule status
        - sudo ufw status verbose

    ![](/img/Assignment8/12.PNG)

    - Verify logging is working
        - sudo tail -f /var/log/ufw.log
    - Test service accessibility
        - nc -zv [server-ip] 22
        - nc -zv [server-ip] 80
        - nc -zv [server-ip] 443

***Rule Order***
    - Rules are processed in order from top to bottom
    - More specific rules are placed before general rules
    - Default policies act as a catch-all for undefined traffic

***Security Considerations***

    - All unnecessary ports remain closed by default
    - Rate limiting on SSH prevents automated attacks
    - SYN flood protection helps maintain service availability
    - Logging enables security monitoring and incident response

***Maintenance Requirements***

    - Regular log review required
    - Update rules as service requirements change
    - Monitor for unusual patterns in blocked traffic

