# SmartDNS

![SmartDNS](doc/smartdns-banner.png)  
SmartDNS is a local DNS server. SmartDNS accepts DNS query requests from local clients, obtains DNS query results from multiple upstream DNS servers, and returns the fastest access results to clients.  
Avoiding DNS pollution and improving network access speed, supports high-performance ad filtering.  
Unlike dnsmasq's all-servers, smartdns returns the fastest access resolution.

Support Raspberry Pi, openwrt, ASUS router and other devices.  

## Software Show

**Ali DNS**  
Use Ali DNS to query Baidu's IP and test the results.  

```shell
pi@raspberrypi:~/code/smartdns_build $ nslookup www.baidu.com 223.5.5.5
Server:         223.5.5.5
Address:        223.5.5.5#53

Non-authoritative answer:
www.baidu.com   canonical name = www.a.shifen.com.
Name:   www.a.shifen.com
Address: 180.97.33.108
Name:   www.a.shifen.com
Address: 180.97.33.107

pi@raspberrypi:~/code/smartdns_build $ ping 180.97.33.107 -c 2
PING 180.97.33.107 (180.97.33.107) 56(84) bytes of data.
64 bytes from 180.97.33.107: icmp_seq=1 ttl=55 time=24.3 ms
64 bytes from 180.97.33.107: icmp_seq=2 ttl=55 time=24.2 ms

--- 180.97.33.107 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 24.275/24.327/24.380/0.164 ms
pi@raspberrypi:~/code/smartdns_build $ ping 180.97.33.108 -c 2
PING 180.97.33.108 (180.97.33.108) 56(84) bytes of data.
64 bytes from 180.97.33.108: icmp_seq=1 ttl=55 time=31.1 ms
64 bytes from 180.97.33.108: icmp_seq=2 ttl=55 time=31.0 ms

--- 180.97.33.108 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 31.014/31.094/31.175/0.193 ms
```

**smartdns**  
Use SmartDNS to query Baidu IP and test the results.

```shell
pi@raspberrypi:~/code/smartdns_build $ nslookup www.baidu.com
Server:         192.168.1.1
Address:        192.168.1.1#53

Non-authoritative answer:
www.baidu.com   canonical name = www.a.shifen.com.
Name:   www.a.shifen.com
Address: 14.215.177.39

pi@raspberrypi:~/code/smartdns_build $ ping 14.215.177.39 -c 2
PING 14.215.177.39 (14.215.177.39) 56(84) bytes of data.
64 bytes from 14.215.177.39: icmp_seq=1 ttl=56 time=6.31 ms
64 bytes from 14.215.177.39: icmp_seq=2 ttl=56 time=5.95 ms

--- 14.215.177.39 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 5.954/6.133/6.313/0.195 ms

```

From the comparison, smartdns found the fastest IP address to visit www.baidu.com, so accessing Baidu's DNS is 5 times faster than Ali DNS.

## Features

1. **Multiple upstream DNS servers**  
   Support configuring multiple upstream DNS servers and query at the same time.the query will not be affected, Even if there is a DNS server exception.  

2. **Return the fastest IP address**  
   Supports finding the fastest access IP address from the IP address list of the domain name and returning it to the client to avoid DNS pollution and improve network access speed.

3. **Support for multiple query protocols**
   Support UDP, TCP, TLS queries, and non-53 port queries, effectively avoiding DNS pollution.

4. **Domain IP address specification**  
   Support configuring IP address of specific domain to achieve the effect of advertising filtering, and avoid malicious websites.

5. **Domain name high performance rule filtering**  
   Support domain name suffix matching mode, simplify filtering configuration, filter 200,000 recording and take time <1ms.

6. **Linux multi-platform support**  
   Support standard Linux system (Raspberry Pi), openwrt system various firmware, ASUS router native firmware.

7. **Support IPV4, IPV6 dual stack**  
   Support IPV4, IPV6 network, support query A, AAAA record.

8. **High performance, low resource consumption**  
   Multi-threaded asynchronous IO mode, cache cache query results.

## Architecture

![Architecture](doc/architecture.png)

1. SmartDNS receives DNS query requests from local network devices, such as PCs and mobile phone query requests.
2. SmartDNS sends query requests to multiple upstream DNS servers, using standard UDP queries, non-standard port UDP queries, and TCP queries.
3. The upstream DNS server returns a list of Server IP addresses corresponding to the domain name. SmartDNS detects the fastest Server IP with local network access.
4. Return the fastest accessed Server IP to the local client.

## Usage

### Download the package

--------------

Download the matching version of the SmartDNS installation package. The corresponding installation package is as follows.

|system |package|Description
|-----|-----|-----
|Standard Linux system (Raspberry Pi)| smartdns.xxxxxxxx.armhf.deb|Support Raspberry Pi Raspbian stretch, Debian 9 system.
|Standard Linux system (x86_64)| smartdns.xxxxxxxx.x86_64.tar.gz|Support for x86_64 systems.
|Standard Linux system (x86)| smartdns.xxxxxxxx.x86.tar.gz|Support for x86_64 systems.
|ASUS native firmware (optware)|smartdns.xxxxxxx.mipsbig.ipk|Systems that support the MIPS big-end architecture, such as RT-AC55U, RT-AC66U.
|ASUS native firmware (optware)|smartdns.xxxxxxx.mipsel.ipk|System that supports the MIPS little endian architecture, such as the RT-AC68U.
|ASUS native firmware (optware)|smartdns.xxxxxxx.arm.ipk|System that supports the ARM small endian architecture, such as the RT-AC88U.
|openwrt 15.01|smartdns.xxxxxxxx.ar71xx.ipk|Support AR71XX MIPS system.
|openwrt 15.01|smartdns.xxxxxxxx.ramips_24kec.ipk|Support small-end routers such as MT762X
|openwrt 15.01(Pandora)|smartdns.xxxxxxxx.mipsel_24kec_dsp.ipk|Support for Pandora firmware of MT7620 series
|openwrt 18.06|smartdns.xxxxxxxx.mips_24kc.ipk|Support AR71XX MIPS system.
|openwrt 18.06|smartdns.xxxxxxxx.mipsel_24kc.ipk|Support small-end routers such as MT726X
|openwrt 18.06|smartdns.xxxxxxxx.x86_64.ipk|Support x86_64 router
|openwrt 18.06|smartdns.xxxxxxxx.i386_pentium4.ipk|Support x86_64 router
|openwrt 18.06|smartdns.xxxxxxxxxxx.arm_cortex-a9.ipk|Router supporting arm A9 core CPU
|openwrt 18.06|smartdns.xxxxxxxxx.arm_cortex-a7_neon-vfpv4.ipk|Router supporting arm A7 core CPU
|openwrt LUCI|luci-app-smartdns.xxxxxxxxx.xxxx.all.ipk|Openwrt management interface

* The openwrt system supports a lot of CPU architecture. The above table does not list all the supported systems. Please check the CPU architecture and download it.
* The merlin Merlin firmware theory is the same as the ASUS firmware, so install the corresponding ipk package according to the hardware type. (Merlin is not verified yet, and has a problem to submit an issue)
* The CPU architecture can be found in the router management interface:  
    Log in to the router, click `System`->`Software`, click the `Configuration` tab page, and find the corresponding software architecture in the opkg installation source. The download path can be found, as follows, the architecture is ar71xx

    ```shell
    src/gz chaos_calmer_base http://downloads.openwrt.org/chaos_calmer/15.05/ar71xx/generic/packages/base
    ```

    Please download it from the Release page: [Download here](https://github.com/pymumu/smartdns/releases)

### Standard Linux system installation (Raspberry Pi, X86_64 system)

--------------

1. Installation

    Download the installation package like `smartdns.xxxxxxxx.armhf.deb` and upload it to the Linux system. Run the following command to install

    ```shell
    dpkg -i smartdns.xxxxxxxx.armhf.deb
    ```

1. Configuration

    After the installation is complete, you can configure the upstream server to  smartdns. Refer to the `Configuration Parameters` for specific configuration parameters.  
    In general, you only need to add `server [IP]:port`, `server-tcp [IP]:port` configuration items.  
    Configure as many upstream DNS servers as possible, including servers at home and abroad. Please refer to the `Configuration Parameters` section for configuration parameters.  

    ```shell
    vi /etc/smartdns/smartdns.conf
    ```

1. Start Service

    ```shell
    systemctl enable smartdns
    systemctl start smartdns
    ```

1. Forwarding DNS request to SmartDNS

    Modify the DNS server of the local router and configure the DNS server as SmartDNS.
    * Log in to the router on the local network and configure the Raspberry Pi to assign a static IP address.
    * Modify the WAN port or DHCP DNS to the Raspberry Pi IP address.
    Note:
    I. Each router configuration method is different. Please search Baidu for related configuration methods.
    II. some routers may not support configuring custom DNS server. in this case, please modify the PC's, mobile phone's DNS server to the ip of Raspberry Pi.

1. Check if the service is configured successfully

    Query domain name with `nslookup -querytype=ptr 127.0.0.1`  
    Check if the `name` item in the command result is displayed as `smartdns` or `hostname`, such as `smartdns`

    ```shell
    pi@raspberrypi:~/code/smartdns_build $ nslookup -querytype=ptr 127.0.0.1
    Server:         192.168.1.1
    Address:        192.168.1.1#53

    Non-authoritative answer:
    1.0.0.127.in-addr.arpa  name = smartdns.
    ```

### openwrt/LEDE

--------------

1. Installation

    Upload the software to the /root directory of the router with winscp or other tool, and execute the following command to install it.

    ```shell
    opkg install smartdns.xxxxxxxx.xxxx.ipk
    opkg install luci-app-smartdns.xxxxxxxx.xxxx.all.ipk
    ```

1. Configuration

    Log in to the openwrt management page and open `Services`->`SmartDNS` to configure SmartDNS.
    * Add upstream DNS server configuration to `Upstream Servers`. It is recommended to configure multiple DNS servers at home and abroad.
    * Specify the IP address of a specific domain name in `Domain Address`, which can be used for ad blocking.

1. Start Service

   There are two ways to use the SmartDNS service, `one is directly as the primary DNS service`, `the other is as the upstream of dnsmasq`.  
   By default, SmartDNS uses the first method. You can choose according to your needs in the following two ways.

1. Method 1: SmartDNS as primary DNS Server (default scheme)

    * **Enable SmartDNS port 53 port redirection**

        Log in to the router, click on `Services`->`SmartDNS`, check the `Redirect` option to enable port 53 forwarding.

    * **Check if the service is configured successfully**

        Query domain name with `nslookup -querytype=ptr 127.0.0.1`
        See if the `name` item in the command result is displayed as `smartdns` or `hostname`, such as `smartdns`

        ```shell
        pi@raspberrypi:~/code/smartdns_build $ nslookup -querytype=ptr 127.0.0.1
        Server:         192.168.1.1
        Address:        192.168.1.1#53

        Non-authoritative answer:
        1.0.0.127.in-addr.arpa  name = smartdns.
        ```

    * **The interface prompts that the redirect failed**

        * Check if iptable, ip6table command is installed correctly.
        * The openwrt 15.01 system does not support IPV6 redirection. If the network needs to support IPV6, please change DNSMASQ upstream to smartdns, or change the smartdns port to 53, and disable dnsmasq.
        * After LEDE system, please install IPV6 nat forwarding driver. Click `system`->`Software`, click `update lists` to update the software list, install `ip6tables-mod-nat`
        * Use the following command to check whether the routing rule takes effect.

        ```shell
        iptables -t nat -L PREROUTING | grep REDIRECT
        ```
        * If the forwarding function is abnormal, please use Method 2: As the upstream of DNSMASQ.

1. Method 2: SmartDNS as upstream DNS Server of DNSMASQ

    * **Disable SmartDNS port 53 port redirection**

        Log in to the router, click on `Services`->`SmartDNS`, uncheck the `Redirect` option to disable port 53 forwarding.

    * **Forward dnsmasq's request to SmartDNS**

        Log in to the router, click `Network`->`DHCP and DNS`, and modify `DNS forwardings` to:

        ```shell
        /#/127.0.0.1#5053
        ```

        Where `#5053` is the service port number of smartdns. If it is not modified, the default is 5053.

    * **Check if the service is configured successfully**

        Use `nslookup` to query the `www.baidu.com` domain name to see if the IP address of Baidu in the result is `only one. If there are multiple IP addresses returned, it means that it is not valid. Please try to check several domain names.

        ```shell
        pi@raspberrypi:~ $ nslookup www.baidu.com 192.168.1.1
        Server:         192.168.1.1
        Address:        192.168.1.1#53

        Non-authoritative answer:
        www.baidu.com   canonical name = www.a.shifen.com.
        Name:   www.a.shifen.com
        Address: 14.215.177.38
        ```

1. Start Service

    Check the `Enable' in the configuration page to start SmartDNS server.

1. Note

    * If chinaDNS is already installed, it is recommended to configure the upstream of chinaDNS as SmartDNS.
    * SmartDNS defaults to forwarding port 53 requests to the local port of SmartDNS, controlled by the `Redirect` configuration option.

### ASUS router native firmware / Merlin firmware

--------------

Note: Merlin firmware is derived from ASUS firmware and can theoretically be used directly with the ASUS package. However, it is currently unverified. If you have any questions, please submit an issue.

1. Prepare

    When using this software, you need to confirm whether the router supports U disk and prepare a USB disk.

1. Enable SSH login

    Log in to the management interface, click `System Management`-> Click `System Settings` and configure `Enable SSH` to `Lan Only`.  
    The SSH login username and password are the same as the management interface.

1. Insstall `Download Master`

    In the management interface, click `USB related application`-> click `Download Master` to download.  
    After the download is complete, enable `Download Master`. If you do not need the download function, you can uninstall `Download Master` here, but make sure that Download Master is enabled before uninstalling.  

1. Install SmartDNS

    Upload the software to the router's `/tmp/mnt/sda1` directory using winscp. (or copy the network neighborhood to the sda1 shared directory)

    ```shell
    ipkg install smartdns.xxxxxxx.mipsbig.ipk
    ```

1. Restart router

    After the router is started, use `nslookup -querytype=ptr 127.0.0.1` to query the domain name.  
    See if the `name` item in the command result is displayed as `smartdns` or `hostname`, such as `smartdns`

    ```shell
    pi@raspberrypi:~/code/smartdns_build $ nslookup -querytype=ptr 127.0.0.1
    Server:         192.168.1.1
    Address:        192.168.1.1#53

    Non-authoritative answer:
    1.0.0.127.in-addr.arpa  name = smartdns.
    ```

1. Note

    In the above process, smartdns will be installed to the root directory of the U disk and run in optware mode.  
    Its directory structure is as follows: (only smartdns related files are listed here)

    ```shell
    USB DISK
    └── asusware.mipsbig
            ├── bin
            ├── etc
            |    ├── smartdns
            |    |     └── smartdns.conf
            |    └── init.d
            |          └── S50smartdns
            ├── lib
            ├── sbin
            ├── usr
            |    └── sbin
            |          └── smartdns
            ....
    ```

    To modify the configuration, you can use ssh to login to the router and use the vi command to modify it.

    ```shell
    vi /opt/etc/smartdns/smartdns.conf
    ```

    It can also be modified from Network Neighborhood. From the neighbor sharing directory `sda1` you can't see the `asusware.mipsbig` directory, but you can directly enter `asusware.mipsbig\etc\init.d` in `File Manager` to modify it.

    ```shell
    \\192.168.1.1\sda1\asusware.mipsbig\etc\init.d
    ```

## Configuration parameter

|parameter|Parameter function|Default value|Value type|Example|
|--|--|--|--|--|
|server-name|DNS name|host name/smartdns|any string like hosname|server-name smartdns
|bind|DNS bind port|[::]:53|IP:PORT|bind 192.168.1.1:53
|bind-tcp|TCP mode DNS bind port|[::]:53|IP:PORT|bind-tcp 192.168.1.1:53
|cache-size|Domain name result cache number|512|integer|cache-size 512
|tcp-idle-time|TCP connection idle timeout|120|integer|tcp-idle-time 120
|rr-ttl|Domain name TTL|Remote query result|number greater than 0|rr-ttl 600
|rr-ttl-min|Domain name Minimum TTL|Remote query result|number greater than 0|rr-ttl-min 60
|rr-ttl-max|Domain name Maximum TTL|Remote query result|number greater than 0|rr-ttl-max 600
|log-level|log level|error|error,warn,info,debug|log-level error
|log-file|log path|/var/log/smartdns.log|File Pah|log-file /var/log/smartdns.log
|log-size|log size|128K|number+K,M,G|log-size 128K
|log-num|archived log number|2|Integer|log-num 2
|conf-file|additional conf file|None|File path|conf-file /etc/smartdns/smartdns.more.conf
|server|Upstream UDP DNS server|None|[ip][:port], Repeatable| server 8.8.8.8:53
|server-tcp|Upstream TCP DNS server|None|[IP][:port], Repeatable| server-tcp 8.8.8.8:53
|server-tls|Upstream TLS DNS server|None|[IP][:port], Repeatable| server-tls 8.8.8.8:853
|address|Domain IP address|None|address /domain/ip| address /www.example.com/1.2.3.4
|bogus-nxdomain|bogus IP address|None|[IP]，Repeatable| bogus-nxdomain 1.2.3.4
|force-AAAA-SOA|force AAAA query return SOA|no|[yes\|no]|force-AAAA-SOA yes

## [Donate](#Donate)  

If you feel that this project is helpful to you, please donate to us so that the project can continue to develop and be more perfect.

### PayPal

[![Support via PayPal](https://cdn.rawgit.com/twolfson/paypal-github-button/1.0.0/dist/button.svg)](https://paypal.me/PengNick/)

### Alipay

![alipay](doc/alipay_donate.jpg)

### Wechat
  
![wechat](doc/wechat_donate.jpg)

## Statement

* `SmartDNS` copyright belongs to Nick Peng (pymumu at gmail.com).
* `SmartDNS` is free software, users can copy and use `SmartDNS` non-commercially.
* It is forbidden to use `SmartDNS` for commercial purposes.
* The use of the software is at the user's own risk and, to the fullest extent permitted by applicable law, damages and risks resulting from the use of this product, including but not limited to direct or indirect personal damage, loss of commercial profit, trade disruption No loss of business information or any other financial loss.
* This software does not collect any user information without the user's consent.

## Additional information

Currently the code is not open source, and subsequent open source according to the situation.
