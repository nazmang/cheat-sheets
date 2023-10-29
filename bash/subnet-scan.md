# subnet scan

In this example, the bash script will scan the subnet for hosts connected at IP addresses 10.1.1.1 – 255.
The script will display the message Host with IP: IP address 'UP' if the ping command was successful.

```shell
#!/bin/bash

is_alive_ping()
{
  ping -c 1 $1 > /dev/null
  [ $? -eq 0 ] && echo Host with IP: $i is UP.
}

for i in 10.1.1.{1..255} 
do
is_alive_ping $i & disown
done
```
Execute:

```console
$ ./bash_ping_scan.sh
```

Output:

```console
Host with IP: 10.1.1.1 is UP.
Host with IP: 10.1.1.4 is UP.
Host with IP: 10.1.1.9 is UP.
```