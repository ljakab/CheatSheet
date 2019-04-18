Capturing on a remote host using the built-in SSH extcap plugin
---------------------------------------------------------------

From the list of available capture interfaces select the gear icon next to
"SSH remote capture: sshdump" to be able to configure the plugin first. Fields
to fill out:
  * Remote SSH server address
  * Remote SSH username
  * (optional) ProxyCommand: ssh -q -W %h:%p jumphost
  * Remote capture command: dumpcap -w - -i ens160 -f 'udp port 4342 or udp port 4789'

It seems that "Remote interface" and "Remote capture filter" are ignored so
their values need to be included in the "Remote capture command".
