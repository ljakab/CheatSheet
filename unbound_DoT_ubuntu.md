# Setting up a DNS-over-TLS server using Unbound on Ubuntu

It can then be used for example on your Android phone as the default DNS
server to override your WiFi or cellular connection's DNS settings.

First you need to have a domain name pointing to the server you want to use
(`dns.example.com` in the following). You then need a valid SSL certificate
for that domain. With Let's Encrypt, you can do this:

    certbot certonly --standalone -d dns.example.com

Running the above will set up a cron entry that will update the certificates
automatically. However, it will not restart all daemons using the
certificates, so the following line needs to be added to the end of
`/etc/letsencrypt/renewal/dns.example.com.conf`:

    renew_hook = systemctl reload unbound

Add the follwing files to `/etc/unbound/unbound.conf.d`:

- `dot-recursive-resolver.conf`

```
server:
    verbosity: 1

    statistics-interval: 3600
    statistics-cumulative: yes
    extended-statistics: yes

    interface: 0.0.0.0@853
    interface: ::0@853

    incoming-num-tcp: 100
    do-udp: no
    udp-upstream-without-downstream: yes
    access-control: 0.0.0.0/0 allow

    #chroot: ""
    logfile: "/var/log/unbound.log"
    log-time-ascii: yes
    log-queries: yes
    log-replies: yes
    log-tag-queryreply: yes
    log-servfail: yes

    hide-identity: yes
    hide-version: yes
    identity: "DNS"

    aggressive-nsec: yes
    prefetch: yes

    tls-service-key: "/etc/letsencrypt/live/dns.example.com/privkey.pem"
    tls-service-pem: "/etc/letsencrypt/live/dns.example.com/fullchain.pem"
    tls-port: 853
    tls-ciphersuites: "TLS_AES_128_GCM_SHA256:TLS_AES_128_CCM_8_SHA256:TLS_AES_128_CCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256"
```

- `remote-control.conf`

```
remote-control:
    control-enable: yes
    control-use-cert: no
```

The remote control bits are needed for **reloading** Unbound instead of
**restarting**. The former just signals the process to re-read it's
configuration file, without stopping and starting it.

By default Unbound is using syslog for logging, and it doesn't log queries.
Our configuration above defines a custom log file, because we want to log all
queries, and don't want to pollute syslog with that. However, the AppArmor
profile for Unbound doesn't allow that by default. To allow it, add the
following to`/etc/apparmor.d/usr.sbin.unbound`:

```
  # Custom log file
  /var/log/unbound.log rw,
```

Additionally, regardless of actual file permissions, AppArmor won't allow
access to the private keys stored by `certbot` under `/etc/letsencrypt/live`.
To fix that, add the following includes to the unbound profile edited before,
and then reload AppArmor:

```
  #include <abstractions/ssl_certs>
  #include <abstractions/ssl_keys>
```
