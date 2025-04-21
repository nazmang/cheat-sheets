# Archive commnds 

## Move each directory to separate archive 
```shell
find . -maxdepth 1 -mindepth 1 -type d -exec tar cvf {}.tar {}  \;
```