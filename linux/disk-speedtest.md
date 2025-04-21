[root@drtdevsamba01 ~]# echo 3 > /proc/sys/vm/drop_caches; dd if=/dev/zero of=/qa/test/testfile.bin bs=1M count=1000 conv=fsync
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 1.66454 s, 630 MB/s
[root@drtdevsamba01 ~]# ll /qa/test/testfile.bin
-rw-r----- 1 root root 1048576000 Dec 22 11:00 /qa/test/testfile.bin
[root@drtdevsamba01 ~]# echo 3 > /proc/sys/vm/drop_caches; dd if=/qa/test/testfile.bin of=/dev/null  bs=1M
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB) copied, 1.03074 s, 1.0 GB/s
