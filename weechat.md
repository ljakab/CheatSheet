Setting up authenticated FreeNode over SSL in Weechat
-----------------------------------------------------

    /set weechat.network.gnutls_ca_file "/etc/ssl/certs/ca-certificates.crt"

    /set irc.server.freenode.autoconnect on
    /set irc.server.freenode.addresses "chat.freenode.net/7000"
    /set irc.server.freenode.ssl on
    /set irc.server.freenode.ssl_dhkey_size 1024

    /set irc.server.freenode.nicks "lori, lori_, lori__"

    /set irc.server.freenode.username "lori"
    /set irc.server.freenode.realname "Lori"

    /set irc.server_default.sasl_mechanism dh-blowfish
    /set irc.server.freenode.sasl_username "lori"
    /set irc.server.freenode.sasl_password "******"

    /set irc.server.freenode.autojoin "#lispmob,#lisp-networking,#openvswitch,#opendaylight"

    /connect freenode

