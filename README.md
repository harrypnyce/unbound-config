# unbound-config
Pi-hole w/ Unbound (tiny) recursive DNS server -- replaces FTLDNS.
> /etc/unbound/unbound.conf.d/pi-hole.conf

> Linux pihole1 5.0.0-29-generic #31~18.04.1-Ubuntu SMP Thu Sep 12 18:29:21 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux

    server:
        # If no logfile is specified, syslog is used
        logfile: "/var/log/unbound/unbound.log"
        verbosity: 2

        port: 5353
        do-ip4: yes
        do-udp: yes
        do-tcp: yes

        # May be set to yes if you have IPv6 connectivity
        do-ip6: no

        # Use this only when you downloaded the list of primary root servers!
        root-hints: "/var/lib/unbound/root.hints"

        # Trust glue only if it is within the servers authority
        harden-glue: yes

        # Require DNSSEC data for trust-anchored zones, if such data is absent, the zone becomes BOGUS
        harden-dnssec-stripped: yes

        # Don't use Capitalization randomization as it known to cause DNSSEC issues sometimes
        # see https://discourse.pi-hole.net/t/unbound-stubby-or-dnscrypt-proxy/9378 for further details
        use-caps-for-id: no

        # Reduce EDNS reassembly buffer size.
        # Suggested by the unbound man page to reduce fragmentation reassembly problems
        edns-buffer-size: 1472

        # Perform prefetching of close to expired message cache entries
        # This only applies to domains that have been frequently queried
        prefetch: yes

        # This attempts to reduce latency by serving the outdated record before
        # updating it instead of the other way around. Alternative is to increase
        # cache-min-ttl to e.g. 3600.
        cache-min-ttl: 0
        serve-expired: yes
        # serve-expired-ttl: 3600 # 0 or not set means unlimited (I think)

        # Use about 2x more for rrset cache, total memory use is about 2-2.5x
        # total cache size. Current setting is way overkill for a small network.
        # Judging from my used cache size you can get away with 8/16 and still
        # have lots of room, but I've got the ram and I'm not using it on anything else.
        # Default is 4m/4m
        msg-cache-size: 128m
        rrset-cache-size: 256m

        # One thread should be sufficient, can be increased on beefy machines.
        num-threads: 1

        # Ensure kernel buffer is large enough to not lose messages in traffic spikes
        so-rcvbuf: 1m

        # Ensure privacy of local IP ranges
        private-address: 192.168.0.0/16
        private-address: 169.254.0.0/16
        private-address: 172.16.0.0/12
        private-address: 10.0.0.0/8
        private-address: fd00::/8
        private-address: fe80::/10

        # Allow this domain, and all its subdomains to contain private addresses.
        private-domain: "lab.example.com"

        # enable remote-control
        remote-control:
        control-enable: yes

    # enable extended statistics.  REQUIRES: <https://www.zabbix.com/documentation/3.4/manual/concepts/sender>
    server:
        statistics-interval: 0
        extended-statistics: yes
        # set to yes if graphing tool needs it
        statistics-cumulative: yes
