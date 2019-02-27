Adding dynv6 as a custom Dynamic DNS provider
---------------------------------------------

Edit `/etc.defaults/ddns_provider.conf` and append the following at the end:

    [dynv6 (IPv4)]
            modulepath=DynDNS
            queryurl=https://ipv4.dynv6.com/api/update?hostname=__HOSTNAME__&ipv4=__MYIP__&token=__PASSWORD__
            website=https://dynv6.com/hosts

NOTE: This will be overwritten when DSM is updated, so it needs to be added
again.

Once this is added, use Control Panel -> External Access -> DDNS to add a new
service provider of type `dynv6 (IPv4)`, where the 'Password/Key' is the token
for dynv6 API access.
