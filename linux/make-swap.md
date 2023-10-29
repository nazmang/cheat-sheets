# Make Swap if not exists
Makes swap on *instances* easy and fast if not exists
```shell
sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=2048
mkswap /var/swap.1 
swapon /var/swap.1
```
```shell
free -h 
              total        used        free      shared  buff/cache   available
Mem:          965Mi       820Mi        70Mi       0.0Ki        73Mi        34Mi
Swap:         2.0Gi       1.3Gi       748Mi
```