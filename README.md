 - [RangeRecon](#RangeRecon): IP whois tool
 - [DomainRecon](#DomainRecon): Domain whois tool
 - [EmailSecurity](#EmailSecurity): Email hosting and security identifier
 - [SSLRecon](#SSLRecon): SSL scanner and CNAME enumerator
 - [ReverseDNS](#ReverseDNS): Reverse DNS scanner
 - [BingIP](#BingIP): Bing IP search

RangeRecon
==========

RangeRecon is an IP whois recon tool.

Pass it IP addresses, CIDR ranges, or domain names and it will display the ASN
info for each IP. This is useful for identifying your target's network ranges.

RangeRecon is multi-threaded and can take IP addresses, CIDR ranges, or domain
names (which it will resolve first).

If possible RangeRecon will use cached results for IP addresses in the same ASNs.

Here's an example:

    $ ./RangeRecon 8.8.8.8 microsoft.com google.com
    [.] Getting ASN info for 104.215.148.63 (microsoft.com)
    [.] Getting ASN info for 40.113.200.201 (microsoft.com)
    [.] Getting ASN info for 8.8.8.8
    [.] Getting ASN info for 13.77.161.179 (microsoft.com)
    [.] Getting ASN info for 40.76.4.15 (microsoft.com)
    [.] Getting ASN info for 40.112.72.205 (microsoft.com)
    [.] Getting ASN info for 172.217.21.238 (google.com)
    [+] Found 5 ASNs
    ASN CIDR: 104.208.0.0/13
    ASN description: MICROSOFT-CORP-MSN-AS-BLOCK - Microsoft Corporation, US
    Network name: MSFT
    Network CIDR(s): 104.208.0.0/13 (same as ASN)
    Contact: Microsoft Corporation
    Hosts:
     - 104.215.148.63 (microsoft.com)
    
    ASN CIDR: 8.8.8.0/24
    ASN description: GOOGLE - Google LLC, US
    Network name: LVLT-GOGL-8-8-8
    Network CIDR(s): 8.8.8.0/24 (same as ASN)
    Contact: Google LLC
    Hosts:
     - 8.8.8.8
    
    ASN CIDR: 13.64.0.0/11
    ASN description: MICROSOFT-CORP-MSN-AS-BLOCK - Microsoft Corporation, US
    Network name: MSFT
    Network CIDR(s): 13.64.0.0/11, 13.96.0.0/13, 13.104.0.0/14
    Contact: Microsoft Corporation
    Hosts:
     - 13.77.161.179 (microsoft.com)
    
    ASN CIDR: 40.64.0.0/10
    ASN description: MICROSOFT-CORP-MSN-AS-BLOCK - Microsoft Corporation, US
    Network name: MSFT
    Network CIDR(s): 40.74.0.0/15, 40.76.0.0/14, 40.80.0.0/12, 40.96.0.0/12, 40.112.0.0/13, 40.120.0.0/14, 40.124.0.0/16, 40.125.0.0/17
    Contact: Microsoft Corporation
    Hosts:
     - 40.113.200.201 (microsoft.com)
     - 40.76.4.15 (microsoft.com)
     - 40.112.72.205 (microsoft.com)
    
    ASN CIDR: 172.217.21.0/24
    ASN description: GOOGLE - Google LLC, US
    Network name: GOOGLE
    Network CIDR(s): 172.217.0.0/16
    Contact: Google LLC
    Hosts:
     - 172.217.21.238 (google.com)

Full list of options:

    $ ./RangeRecon -h
    usage: RangeRecon [-h] [-f FILE] [-n NAMESERVER] [--system-nameservers] [-u]
                      [-t MAX_THREADS] [-o OUTPUT] [-q] [-D]
                      [item [item ...]]
    
    optional arguments:
      -h, --help            show this help message and exit
    
    input arguments:
      item                  IP address, CIDR range, or domain name to perform
                            recon on
      -f FILE, --file FILE  file containing IP addresses, CIDR ranges, and/or
                            domain names to perform recon on
    
    dns arguments:
      -n NAMESERVER, --nameserver NAMESERVER
                            use these nameservers (default: 8.8.8.8)
      --system-nameservers  use system nameservers
      -u, --udp             use udp instead of tcp to resolve domains
    
    threading arguments:
      -t MAX_THREADS, --max-threads MAX_THREADS
                            maximum number of threads to use at once (default: 6)
    
    output arguments:
      -o OUTPUT, --output OUTPUT
                            tee output to a file
      -q, --quiet           do not print status messages to stderr
      -D, --debug           enable debug output

DomainRecon
===========

DomainRecon is a multi-threaded whois recon tool.

Pass it domain names and it will display condensed whois info for each domain.
DomainRecon will clean up domains with subdomains and extract domains from
URLs.

Here's an example:

    $ ./DomainRecon api.github.com http://youtube.com amazon.co.uk
    [.] Getting whois info for amazon.co.uk
    [.] Getting whois info for http://youtube.com
    [.] Getting whois info for api.github.com
    [+] Got whois info for 3 domains
    Domain: amazon.co.uk
    Registrar: Amazon.com, Inc. t/a Amazon.com, Inc. [Tag = AMAZON-COM]
    Creation: before Aug-1996
    Expiration: 2020-12-05 00:00:00
    Name servers: ns1.p31.dynect.net
    
    Domain: http://youtube.com
    Registrar: MarkMonitor, Inc.
    Organization: Google LLC
    Emails: abusecomplaints@markmonitor.com, whoisrequest@markmonitor.com
    Country: US
    Creation: 2005-02-14 21:13:12, 2005-02-15 05:13:12
    Expiration: 2020-02-14 00:00:00, 2020-02-15 05:13:12
    Name servers: ns1.google.com, ns2.google.com, ns3.google.com, ns4.google.com
    
    Domain: api.github.com
    Registrar: MarkMonitor Inc.
    Emails: abusecomplaints@markmonitor.com
    Creation: 2007-10-09 18:20:50
    Expiration: 2020-10-09 18:20:50
    Name servers: ns-1283.awsdns-32.org, ns-1707.awsdns-21.co.uk, ns-421.awsdns-52.com, ns-520.awsdns-01.net, ns1.p16.dynect.net, ns2.p16.dynect.net, ns3.p16.dynect.net, ns4.p16.dynect.net

Full list of options:

    $ ./DomainRecon -h
    usage: DomainRecon [-h] [-f FILE] [-t MAX_THREADS] [-o OUTPUT] [-q] [-D]
                       [domain [domain ...]]
    
    optional arguments:
      -h, --help            show this help message and exit
    
    input arguments:
      domain                Domain name
      -f FILE, --file FILE  file containing domain names
    
    threading arguments:
      -t MAX_THREADS, --max-threads MAX_THREADS
                            maximum number of threads to use at once (default: 6)
    
    output arguments:
      -o OUTPUT, --output OUTPUT
                            tee output to a file
      -q, --quiet           do not print status messages to stderr
      -D, --debug           enable debug output

EmailSecurity
=============

EmailSecurity enumerates domains' mail hosting and security providers.

Example:

    $ ./EmailSecurity google.com mail2tor.com facebook.com example.com microsoft.com proofpoint.com mimecast.com
    proofpoint.com: proofpoint
    microsoft.com: office365
    google.com: google
    mail2tor.com: unknown (91.234.99.184)
    example.com failed: The DNS response does not contain an answer to the question: example.com. IN MX
    facebook.com: self-hosted
    mimecast.com: mimecast

Verbose output:

    $ ./EmailSecurity --verbose facebook.com proofpoint.com
    facebook.com: self-hosted (smtpin.vvv.facebook.com)
    proofpoint.com: proofpoint (mx0a-00148501.pphosted.com mx0b-00148501.pphosted.com mxb-00148501.gslb.pphosted.com mxa-00148501.gslb.pphosted.com)

EmailSecurity is multi-threaded and can take email addresses as well as domains. For
more options:

    $ ./EmailSecurity -h
    usage: EmailSecurity [-h] [-f FILE] [-t MAX_THREADS] [-n NAMESERVER]
                         [--system-nameserver] [-T] [--highlight-security]
                         [--ignore-failed] [-o OUTPUT] [-q] [-v] [-D]
                         [item [item ...]]
    
    optional arguments:
      -h, --help            show this help message and exit
    
    input arguments:
      -f FILE, --file FILE  file containing domains or email addresses to check
      item                  domain or email address to check
    
    threading arguments:
      -t MAX_THREADS, --max-threads MAX_THREADS
                            maximum number of checking threads (default: 50)
    
    dns arguments:
      -n NAMESERVER, --nameserver NAMESERVER
                            use these nameservers (default: 8.8.8.8)
      --system-nameserver   use system nameserver
      -T, --tcp             use tcp instead of udp for dns
    
    output arguments:
      --highlight-security  highlight security hosters
      --ignore-failed       do not show failed queries
      -o OUTPUT, --output OUTPUT
                            tee output to a file
      -q, --quiet           do not print status messages to stderr
      -v, --verbose         verbose mode
      -D, --debug           debug mode

SSLRecon
========

SSLRecon is an SSL scanner.

Pass it IP addresses, CIDR ranges, or domain names (with optional port ranges)
and it will enumerate SSL certificate information on each host.

    $ ./SSLRecon --verbose 40.76.4.0/24
    ...
    Host: 40.76.4.37:443
    Subject: CN=www.nailereviews.com
    Issuer: C=US O=Let's Encrypt CN=Let's Encrypt Authority X3
    MD5: C3:09:F0:C3:8A:8F:E7:33:DB:96:5D:38:A1:1A:A6:DE
    SHA1: AB:C7:6F:52:46:AE:5F:8D:55:3B:48:3C:8F:46:12:1B:4D:0C:86:57
    
    Host: 40.76.4.35:443
    Subject: CN=servicebus.windows.net
    Issuer: C=US ST=Washington L=Redmond O=Microsoft Corporation OU=Microsoft IT CN=Microsoft IT TLS CA 5
    MD5: 15:21:60:9D:E9:4C:ED:53:99:1C:E2:9E:B7:DF:07:E1
    SHA1: C9:9A:0D:60:8F:E7:AB:94:4B:C7:2D:70:AC:67:75:77:CE:34:8B:50
    
    Host: 40.76.4.52:443
    Subject: CN=*.dev01.conceptshare.com
    Issuer: C=US O=Let's Encrypt CN=Let's Encrypt Authority X3
    MD5: 90:48:74:40:EA:D4:24:D8:24:82:60:5A:41:1D:86:E2
    SHA1: B9:6D:23:42:75:DC:13:8F:07:12:85:1D:B2:35:53:FE:DA:90:90:63
    ...

SSLRecon can also list CNAMEs for each host. This is useful for finding web
server hostnames on your target network.

    $ ./SSLRecon --cnames 40.76.4.0/24
    ...
    40.76.4.53:443 *.search.windows.net
    40.76.4.52:443 *.dev01.conceptshare.com
    40.76.4.77:443 cloudflare origin certificate
    40.76.4.81:443 *.mortgagealliance.com
    40.76.4.106:443 ci-rds.cloudapp.net
    40.76.4.84:443 int-04-eus.ivr.mediapaas.skype.net
    ...

For more options:

    $ ./SSLRecon -h
    usage: SSLRecon [-h] [-f FILE] [-n NAMESERVER] [--system-nameservers] [-T]
                    [-t MAX_THREADS] [--cnames] [-o OUTPUT] [-v] [-q]
                    [-D]
                    [item [item ...]]
    
    optional arguments:
      -h, --help            show this help message and exit
    
    input arguments:
      item                  file containing ip[:port], domain[:port], or
                            ip/cidr[:portrange]. portrange is an optional range of
                            ports to check where "443,8441-8443" produces ports
                            443, 8441, 8442, and 8443. by default SSLRecon checks
                            port 443
      -f FILE, --file FILE  file containing items, one on each line. see "item"
                            argument
    
    dns arguments:
      -n NAMESERVER, --nameserver NAMESERVER
                            use these nameservers (default: 8.8.8.8)
      --system-nameservers  use system nameservers
      -T, --tcp             use tcp instead of udp for dns
    
    threading arguments:
      -t MAX_THREADS, --max-threads MAX_THREADS
                            maximum number of threads to use at once (default:
                            200)
    
    output arguments:
      --realtime            print results in realtime. recommended for large
                            ranges
      --cnames              only print cnames (good for finding web servers)
      -o OUTPUT, --output OUTPUT
                            tee output to a file
      -v, --verbose         verbose output (show fingerprint)
      -q, --quiet           do not print status messages to stderr
      -D, --debug           enable debug output

ReverseDNS
==========

ReverseDNS is a multi-threaded reverse DNS scanner.

ReverseDNS can take IP addresses, CIDR ranges, or domain names
(to search for sibling domains).

Here's an example:

    $ ./ReverseDNS 192.30.253.0/24 facebook.com
    [+] Found 44 PTR records
    192.30.253.122: lb-192-30-253-122-iad.github.com
    192.30.253.128: lb-192-30-253-128-iad.github.com
    31.13.71.36 (facebook.com): edge-star-mini-shv-01-lga3.facebook.com
    192.30.253.2: lb-192-30-253-2-iad.github.com
    ...

Full list of options:

    $ ./ReverseDNS -h
    usage: ReverseDNS [-h] [-f FILE] [-n NAMESERVER] [--system-nameservers] [-T]
                      [-t MAX_THREADS] [--realtime] [-o OUTPUT] [-q] [-D]
                      [item [item ...]]
    
    optional arguments:
      -h, --help            show this help message and exit
    
    input arguments:
      item                  IP address, CIDR range, or domain name to perform
                            recon on
      -f FILE, --file FILE  file containing IP addresses, CIDR ranges, and/or
                            domain names to perform recon on
    
    dns arguments:
      -n NAMESERVER, --nameserver NAMESERVER
                            use these nameservers (default: 8.8.8.8)
      --system-nameservers  use system nameservers
      -T, --tcp             use tcp instead of udp for dns
    
    threading arguments:
      -t MAX_THREADS, --max-threads MAX_THREADS
                            maximum number of threads to use at once (default:
                            200)
    
    output arguments:
      --realtime            print results in realtime. recommended for large
                            ranges
      -o OUTPUT, --output OUTPUT
                            tee output to a file
      -q, --quiet           do not print status messages to stderr
      -D, --debug           enable debug output

Quick note regarding Tor: You need to use the `--tcp` flag when using
ReverseDNS over Tor. Tor's DNS cache (which is used when you make a UDP DNS
query over Tor) is broken and caches PTR queries as `REVERSE[*.in-addr.arpa.]`
instead of `*.in-addr.arpa.` (see
[here](https://github.com/torproject/tor/blob/master/src/core/or/connection_edge.c#L1703)).
When a query is satisfied from Tor's DNS cache the answer field in the DNS
response is set correctly but the question field contains
`REVERSE[*.in-addr.arpa.]`. The dnspython library only returns answers that
match the query name exactly so ReverseDNS breaks with Tor's non-standard DNS
responses.

BingIP
======

**Note: Bing seems to have killed this. Lame.**

BingIP resolves IP addresses to their associated domain names using Bing's
[`ip:<ip address>` queries](https://searchenginewatch.com/sew/news/2051946/bing-allows-search-domains-single-ip-address).

BingIP is multi-threaded and can take IP addresses, CIDR ranges, or domain
names (to search for sibling domains).

Here's an example:

    $ ./BingIP github.com 40.76.4.15
    [.] Performing Bing search for IP 192.30.253.113 (github.com)
    [.] Performing Bing search for IP 192.30.253.112 (github.com)
    [.] Performing Bing search for IP 40.76.4.15
    [+] Found 8 results
    40.76.4.15: microsoft.com, powershellgallery.com, www.research.microsoft.com, answers.microsoft.com, msdn.microsoft.com, www.xbox.com
    192.30.253.112 (github.com): github.com
    192.30.253.113 (github.com): github.com

Full list of options:

    $ ./BingIP -h
    usage: BingIP [-h] [-f FILE] [-p PROXY] [-A USER_AGENT] [-n NAMESERVER]
                  [--system-nameservers] [-u] [-t MAX_THREADS] [-o OUTPUT] [-q]
                  [-D]
                  [item [item ...]]
    
    optional arguments:
      -h, --help            show this help message and exit
    
    input arguments:
      item                  IP address, CIDR range, or domain name to perform
                            recon on
      -f FILE, --file FILE  file containing IP addresses, CIDR ranges, and/or
                            domain names to perform recon on
    
    http arguments:
      -p PROXY, --proxy PROXY
                            use proxy
      -A USER_AGENT, --user-agent USER_AGENT
                            user-agent for HTTP requests (default: Mozilla/4.0
                            (Windows NT 6.3; Win64; x64) AppleWebKit/537.36
                            (KHTML, like Gecko) Chrome/11.0.1245.0 Safari/537.36)
    
    dns arguments:
      -n NAMESERVER, --nameserver NAMESERVER
                            use these nameservers (default: 8.8.8.8)
      --system-nameservers  use system nameservers
      -u, --udp             use udp instead of tcp to resolve domains
    
    threading arguments:
      -t MAX_THREADS, --max-threads MAX_THREADS
                            maximum number of threads to use at once (default: 6)
    
    output arguments:
      -o OUTPUT, --output OUTPUT
                            tee output to a file
      -q, --quiet           do not print status messages to stderr
      -D, --debug           enable debug output
