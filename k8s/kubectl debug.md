The `kubectl debug` command allows you to debug Pods in Kubernetes by creating temporary containers for diagnostics or by running a Pod in a special mode.

### **Primary Uses of `kubectl debug`**

#### **1. Running an Ephemeral Container for Debugging**
If a Pod is running, but you need to inspect its state, you can add a temporary container with debugging tools (e.g., `busybox`, `alpine`, `netshoot`):

```shell
kubectl debug <pod-name> -it --image=busybox --target=<container-name>
````

- `-it` – Interactive mode with TTY.
- `--image` – Image for debugging (e.g., `alpine`, `nicolaka/netshoot`).
- `--target` – The container to attach to (useful if you need to share the process namespace).

> **⚠️ Ephemeral containers cannot be modified after creation!**

---

#### **2. Running a Copy of a Pod with Debugging Changes**

If the Pod isn't starting, you can create a copy of it, adding debugging tools or changing its startup command:

```shell
kubectl debug <pod-name> -it --copy-to=<debug-pod-name> --image=alpine --share-processes
```

- `--copy-to` – Creates a copy of the Pod with a new name.
- `--share-processes` – Allows viewing processes from the original container (useful for `ps aux`, `strace`).

**Example of changing the command:**

```shell
kubectl debug <pod-name> --copy-to=<debug-pod-name> --container=<container-name> -- sh
```

(Replaces the container's command with `sh` for interactive access).

---

#### **3. Node-Level Debugging**

If the problem seems related to the Node itself, you can launch a Pod with privileged access to the node:

```shell
kubectl debug node/<node-name> -it --image=alpine
```

(This launches a Pod with access to the node's filesystem).

> **⚠️ Requires `cluster-admin` privileges!**

---

### **Useful Examples**

|   |   |
|---|---|
|**Command**|**Description**|
|`kubectl debug my-pod -it --image=nicolaka/netshoot`|Debug networking within the Pod.|
|`kubectl debug my-pod --copy-to=my-pod-debug --share-processes`|Create a Pod copy with shared process namespace.|
|`kubectl debug my-pod -it --image=alpine --target=app`|Attach an ephemeral container sharing the target container's process namespace.|
|`kubectl debug node/my-node -it --image=alpine`|Access the node's filesystem and environment.|

### **Summary**

- For interactive debugging, use `kubectl debug -it --image=...`.
- If a Pod isn't starting, create a copy using `--copy-to`.
- For in-depth debugging, consider using `--share-processes` and `--target`.

Useful images for debugging:

- `busybox` (minimalist Unix utilities)
- `alpine` (includes the `apk` package manager)
- `nicolaka/netshoot` (network utilities like `tcpdump`, `netstat`, `curl`)
- `ubuntu` (if you need `apt`).

If `kubectl debug` isn't working, ensure that **Ephemeral Containers** support is enabled in your Kubernetes cluster (generally available from Kubernetes v1.23+).
