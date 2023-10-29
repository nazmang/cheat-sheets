# New Repository existing data conflict
If you see something like below 
```shell
 You have divergent branches and need to specify how to reconcile them.
 You can do so by running one of the following commands sometime before
 your next pull:
 
   git config pull.rebase false  # merge
   git config pull.rebase true   # rebase
   git config pull.ff only       # fast-forward only
 
 You can replace "git config" with "git config --global" to set a default
 preference for all repositories. You can also pass --rebase, --no-rebase,
 or --ff-only on the command line to override the configured default per
 invocation.
```
Run locally after commit 
```shell
git pull origin main --rebase
```
