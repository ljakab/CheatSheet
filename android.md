* Determine DNS servers in the CLI

    getprop net.dns1
    getprop net.dns2
    ...

* Get a mapping of app name to user ID on the CLI

    dumpsys package | awk '/Package \[/{ gsub(/\$|\[|\]/, ""); getline t; sub(/.*userId=/, " ", t); print $2 t }'
