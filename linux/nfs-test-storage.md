# NFS and storage performance test

- dd – without conv=fsync or oflag=direct. This uses filesystem cache.
- scp – This is bottlenecked by a single CPU core performing encryption, and eventually by OpenSSH’s built-in 64KiB buffer size.
- rsync – This uses filesystem cache, and cannot bypass the cache.

> **These tests examples will drop filesystem cache. Please use them carefully!**

## Test write performance of the local storage with dd command

```shell
echo 3 > /proc/sys/vm/drop_caches
dd if=/dev/zero of=/tmp/testfile.bin bs=1M count=10000 conv=fsync
```

## Test read performance of the local storage with dd command

```shell
echo 3 > /proc/sys/vm/drop_caches
dd if=/tmp/testfile.bin of=/dev/null bs=1M
```

## Test streaming write performance of the NFS client with dd command

```shell
echo 3 > /proc/sys/vm/drop_caches
dd if=/dev/zero of=/mnt/nfs/testfile.bin bs=1M count=10000 conv=fsync
```

## Test streaming read performance of NFS client with dd command

```shell
echo 3 > /proc/sys/vm/drop_caches
dd if=/mnt/nfs/testfile.bin of=/dev/null bs=1M
```
