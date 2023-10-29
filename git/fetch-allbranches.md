# How do I fetch all Git branches?
Run the first command only if there are remote branches on the server that aren't tracked by your local branches.
```shell
git branch -r | grep -v '\->' | sed "s,\x1B\[[0-9;]*[a-zA-Z],,g" | while read remote; do git branch --track "${remote#origin/}" "$remote"; done
git fetch --all
git pull --all
```