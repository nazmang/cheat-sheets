# Curl Useful Commands
### Test download speed

```shell
curl -k https://example.com/big_file.dat -w '\nResponse size:\t%{size_download} bytes\nDownload speed:\t%{speed_download} bps\nTotal Time:\t%{time_total} s\n' -o /dev/null

 % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 12.5M  100 12.5M    0     0   654k      0  0:00:19  0:00:19 --:--:--  748k

Response size:	13117780 bytes
Download speed:	669824 bps
Total Time:	19.583904 s
```