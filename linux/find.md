# Using the `find` command

## Find and replace a text in files recursively

```shell
find /home/www \( -type d -name .git -prune \) -o -type f -print0 | xargs -0 sed -i 's/text/substitution/g'
```
