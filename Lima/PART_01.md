# PART 01
---
# September 30, 2022
## Update `lima`
I had an old version of `lima` installed so the first step was to update it.

```bash
## Update lima to a more recent version
brew upgrade lima
==> Upgrading 1 outdated package:
lima 0.6.3 -> 0.11.1

## Start lima-vm
limactl start default

## Watch the VM startup logs
Ctrl+T
tail -f /Users/mac/.lima/default/serial.log

limactl start default
INFO[0000] Using the existing instance "default"
INFO[0000] Downloading "https://github.com/containerd/nerdctl/releases/download/v0.11.1/nerdctl-full-0.11.1-linux-amd64.tar.gz" (sha256:ce7a6e119b03c3fb8ded3d46d929962fd17417bea1d5bbc07e0fce49494d8a09)
INFO[0000] Using cache "/Users/mac/Library/Caches/lima/download/by-url-sha256/3304d173f1e1824e5be6cf84bf2f78825cf0db416c4c975dbb2458776942629e/data"
INFO[0004] [hostagent] Starting QEMU (hint: to watch the boot progress, see "/Users/mac/.lima/default/serial.log")
INFO[0004] SSH Local Port: 60022
```

## Use `lima` for `docker compose` - 1st attempt
While starting up a `docker-compose.yml` file using `lima nerdctl compose up`, I encountered the following (harmless?) output at the end:
```bash
unpacking docker.io/library/tc-online-review:latest (sha256:19411a432b51cc45f804bd2552b1022347b8aa92e80ad8a2d188b768a5be91ad)...done
INFO[0387] Creating container tc-online-review_tc-fakesmtp_1
INFO[0387] Creating container tc-online-review_tc-cache_1
INFO[0387] Creating container tc-online-review_tc-online-review_1
INFO[0387] Creating container tc-online-db
FATA[0001] failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running hook #0: error running hook: exit status 1, stdout: , stderr: time="2022-09-30T16:18:12Z" level=fatal msg="failed to call cni.Setup: plugin type=\"isolation\" failed (add): incompatible CNI versions; config is \"1.0.0\", plugin supports [\"0.1.0\" \"0.2.0\" \"0.3.0\" \"0.3.1\" \"0.4.0\"]"
Failed to write to log, write /home/mac.linux/.local/share/nerdctl/1935db59/containers/default/1ea56cb5546d20cf154b3f8618de7f09c07b018a87e22e240162370a945431b1/oci-hook.createRuntime.log: file already closed: unknown
FATA[0001] failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running hook #0: error running hook: exit status 1, stdout: , stderr: time="2022-09-30T16:18:12Z" level=fatal msg="failed to call cni.Setup: plugin type=\"isolation\" failed (add): incompatible CNI versions; config is \"1.0.0\", plugin supports [\"0.1.0\" \"0.2.0\" \"0.3.0\" \"0.3.1\" \"0.4.0\"]"
Failed to write to log, write /home/mac.linux/.local/share/nerdctl/1935db59/containers/default/380645c6215321b2ccf71ba1078d34cc501de91adf4cc8128edf65a8934ec3fb/oci-hook.createRuntime.log: file already closed: unknown
FATA[0001] failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running hook #0: error running hook: exit status 1, stdout: , stderr: time="2022-09-30T16:18:12Z" level=fatal msg="failed to call cni.Setup: plugin type=\"isolation\" failed (add): incompatible CNI versions; config is \"1.0.0\", plugin supports [\"0.1.0\" \"0.2.0\" \"0.3.0\" \"0.3.1\" \"0.4.0\"]"
Failed to write to log, write /home/mac.linux/.local/share/nerdctl/1935db59/containers/default/7bb128776e1f27d569b53c27c1964506ff352f9e2ec1ebad16cc421c0c580e78/oci-hook.createRuntime.log: file already closed: unknown
FATA[0001] failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running hook #0: error running hook: exit status 1, stdout: , stderr: time="2022-09-30T16:18:12Z" level=fatal msg="failed to call cni.Setup: plugin type=\"isolation\" failed (add): incompatible CNI versions; config is \"1.0.0\", plugin supports [\"0.1.0\" \"0.2.0\" \"0.3.0\" \"0.3.1\" \"0.4.0\"]"
Failed to write to log, write /home/mac.linux/.local/share/nerdctl/1935db59/containers/default/7d5c186d9bea1c3f714ab7fcaeedade57c9c06139aeb059f8acb393e75be5b2c/oci-hook.createRuntime.log: file already closed: unknown
FATA[0388] error while creating container tc-online-review_tc-fakesmtp_1: exit status 1
```
Searching on the errors:
- `oci-hook.createRuntime.log: file already closed: unknown` - [workaround](https://github.com/containerd/nerdctl/issues/665#issuecomment-1040097772) is to do `lima nerdctl start <containerhash>`
- `failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running hook #0: error running hook: exit status 1,`

This led me to suspect it might be a timeout issue. The default timeout might be sufficient assuming the default kind of IPC between the docker cli and the docker engine, but in the case of `lima` and [nVidia's](https://github.com/NVIDIA/nvidia-docker) docker implementation for GPU acceleration, there are multiple IPC layers which is why it [times out](https://github.com/NVIDIA/nvidia-docker/issues/1648#issuecomment-1161813687), so the timeout needs to be higher.

## Use `lima` for `docker compose` - 2nd attempt
Re-running `lima nerdctl compose up` again the error persists but is more informative:
```
INFO[0000] Ensuring image redis:3.2.10
INFO[0000] Ensuring image munkyboy/fakesmtp
INFO[0000] Ensuring image tc-informix:latest
INFO[0000] Ensuring image tc-online-review:latest
INFO[0000] Creating container tc-online-review_tc-fakesmtp_1
INFO[0000] Creating container tc-online-review_tc-cache_1
INFO[0000] Creating container tc-online-db
INFO[0000] Creating container tc-online-review_tc-online-review_1
FATA[0000] failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running hook #0: error running hook: exit status 1, stdout: , stderr: time="2022-09-30T17:05:14Z" level=fatal msg="failed to call cni.Setup: plugin type=\"isolation\" failed (add): incompatible CNI versions; config is \"1.0.0\", plugin supports [\"0.1.0\" \"0.2.0\" \"0.3.0\" \"0.3.1\" \"0.4.0\"]"
Failed to write to log, write /home/mac.linux/.local/share/nerdctl/1935db59/containers/default/328d0e99f22bc11faf046e41c747d53d445aaf0fd43c8a67ac50470bd6af263f/oci-hook.createRuntime.log: file already closed: unknown
FATA[0001] failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running hook #0: error running hook: exit status 1, stdout: , stderr: time="2022-09-30T17:05:14Z" level=fatal msg="failed to call cni.Setup: plugin type=\"isolation\" failed (add): incompatible CNI versions; config is \"1.0.0\", plugin supports [\"0.1.0\" \"0.2.0\" \"0.3.0\" \"0.3.1\" \"0.4.0\"]"
Failed to write to log, write /home/mac.linux/.local/share/nerdctl/1935db59/containers/default/134bd23af6b1ffe1e71a0de83e6ccd79f510802a2b7793a60772e535bf885d23/oci-hook.createRuntime.log: file already closed: unknown
FATA[0001] failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running hook #0: error running hook: exit status 1, stdout: , stderr: time="2022-09-30T17:05:14Z" level=fatal msg="failed to call cni.Setup: plugin type=\"isolation\" failed (add): incompatible CNI versions; config is \"1.0.0\", plugin supports [\"0.1.0\" \"0.2.0\" \"0.3.0\" \"0.3.1\" \"0.4.0\"]"
Failed to write to log, write /home/mac.linux/.local/share/nerdctl/1935db59/containers/default/077e4816b1f7a04b7bab73c02c8021b5fb81b42841486d56319ee53cc29786fa/oci-hook.createRuntime.log: file already closed: unknown
FATA[0001] failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: error during container init: error running hook #0: error running hook: exit status 1, stdout: , stderr: time="2022-09-30T17:05:14Z" level=fatal msg="failed to call cni.Setup: plugin type=\"isolation\" failed (add): incompatible CNI versions; config is \"1.0.0\", plugin supports [\"0.1.0\" \"0.2.0\" \"0.3.0\" \"0.3.1\" \"0.4.0\"]"
Failed to write to log, write /home/mac.linux/.local/share/nerdctl/1935db59/containers/default/af58b1c4a3e40911eed7d62c2b726296f2a49b3d2aad1d96e7a5dbfe79d2c6b1/oci-hook.createRuntime.log: file already closed: unknown
FATA[0001] error while creating container tc-online-review_tc-fakesmtp_1: exit status 1
```
The issue seems to be: `failed (add): incompatible CNI versions; config is 1.0.0, plugin supports 0.1.0` which led me to this [discussion](https://github.com/containerd/nerdctl/discussions/1129).

```bash
lima nerdctl version

Client:
 Version: v0.21.0
 OS/Arch: linux/amd64
 Git commit:  9ddf5226eabcbb7b4b43987f3b0f8d53d86d3bca

Server:
 containerd:
  Version:  v1.6.6
  GitCommit:  10c12954828e7c7c9b6e0ea9b0c02b01407d3ae1

mac@13-inch-MacBook-Pro ~ % lima
mac@lima-default:/Users/mac$ top
top - 17:12:40 up  3:29,  1 user,  load average: 0.10, 0.08, 0.06
Tasks: 137 total,   1 running, 136 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.2 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   3920.6 total,    299.4 free,    269.1 used,   3352.1 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.   3357.7 avail Mem

...
mac@lima-default:/Users/mac$ free
              total        used        free      shared  buff/cache   available
Mem:        4014644      275244      306840        1080     3432560     3438548
Swap:             0           0           0
mac@lima-default:/Users/mac$ sudo -s
```

Checking the CLI options of `nerdctl` for `--cni-path string` by executing:
```
lima nerdctl | grep -e "--cni-path string"
```
gave the following output:
```
cni plugins binary directory [$CNI_PATH] (default "/usr/local/libexec/cni")
``` 
so I checked the path on the `lima` VM directly:
```bash
mac@13-inch-MacBook-Pro ~ % lima
mac@lima-default:/Users/mac$
mac@lima-default:/Users/mac$ ll /usr/local/libexec/cni -v
total 69348
drwxrwxr-x 2 root root    4096 Jun 18 16:17 ./
drwxr-xr-x 3 root root    4096 Jun 18 16:17 ../
-rwxr-xr-x 1 root root 3780654 Mar  9  2022 bandwidth*
-rwxr-xr-x 1 root root 4221977 Mar  9  2022 bridge*
-rwxr-xr-x 1 root root 9742834 Mar  9  2022 dhcp*
-rwxr-xr-x 1 root root 4345726 Mar  9  2022 firewall*
-rwxr-xr-x 1 root root 3357992 Feb  5  2021 flannel*
-rwxr-xr-x 1 root root 3811793 Mar  9  2022 host-device*
-rwxr-xr-x 1 root root 3241605 Mar  9  2022 host-local*
-rwxr-xr-x 1 root root 3922560 Mar  9  2022 ipvlan*
-rwxr-xr-x 1 root root 2387968 Jan 19  2021 isolation*   <================= modify date is different; older file?
-rwxr-xr-x 1 root root 3295519 Mar  9  2022 loopback*
-rwxr-xr-x 1 root root 3959868 Mar  9  2022 macvlan*
-rwxr-xr-x 1 root root 3679140 Mar  9  2022 portmap*
-rwxr-xr-x 1 root root 4092460 Mar  9  2022 ptp*
-rwxr-xr-x 1 root root 3484284 Mar  9  2022 sbr*
-rwxr-xr-x 1 root root 2818627 Mar  9  2022 static*
-rwxr-xr-x 1 root root 3379564 Mar  9  2022 tuning*
-rwxr-xr-x 1 root root 3920827 Mar  9  2022 vlan*
-rwxr-xr-x 1 root root 3523475 Mar  9  2022 vrf*
```


# October 1, 2022
## Use `lima` for `docker compose` - 3rd attempt
Current steps outlined:
```bash
# 1st & 2nd attempts failed. 
lima nerdctl compose up 

lima nerdctl ps --all

# remove dangling containers
lima nerdctl rm $(lima nerdctl ps -qa)

# remove 13-month old container images
lima nerdctl rmi 38c 6c7 ab6 728 5dc e1 85

# stop VM
limactl stop default

# list VMs -or limactl list
limactl ls -q
-> default
```

The next command will re-create the lima VM from SCRATCH *⚠️ REMEMBER TO BACKUP FILES ⛔️*
```bash
# remove all VMs -or limactl delete --all
limactl rm -f $(limactl ls -q)

INFO[0000] Sending SIGKILL to the qemu driver process 14374
INFO[0000] Sending SIGKILL to the host agent process 14320
INFO[0000] Removing *.pid *.sock under "/Users/mac/.lima/default"
INFO[0000] Removing "/Users/mac/.lima/default/ga.sock"
INFO[0000] Removing "/Users/mac/.lima/default/ha.pid"
INFO[0000] Removing "/Users/mac/.lima/default/ha.sock"
INFO[0000] Removing "/Users/mac/.lima/default/qemu.pid"
INFO[0000] Removing "/Users/mac/.lima/default/qmp.sock"
INFO[0000] Removing "/Users/mac/.lima/default/serial.sock"
INFO[0000] Removing "/Users/mac/.lima/default/ssh.sock"
INFO[0000] Deleted "default" ("/Users/mac/.lima/default")
```
Create a new `lima` VM.
```bash
limactl start default
```

## 3rd attempt
After re-creating another `lima` VM instance, I make another attempt.
```bash
limactl start default

? Creating an instance "default" Open an editor to review or modify the current configuration
INFO[0108] Attempting to download the image from "https://cloud-images.ubuntu.com/releases/22.04/release-20220420/ubuntu-22.04-server-cloudimg-amd64.img"  digest="sha256:de5e632e17b8965f2baf4ea6d2b824788e154d9a65df4fd419ec4019898e15cd"
INFO[0120] Attempting to download the image from "https://cloud-images.ubuntu.com/releases/22.04/release/ubuntu-22.04-server-cloudimg-amd64.img"  digest=
630.38 MiB / 630.38 MiB [-----------------------------------] 100.00% 1.59 MiB/s
INFO[0518] Downloaded the image from "https://cloud-images.ubuntu.com/releases/22.04/release/ubuntu-22.04-server-cloudimg-amd64.img"
INFO[0519] Attempting to download the nerdctl archive from "https://github.com/containerd/nerdctl/releases/download/v0.21.0/nerdctl-full-0.21.0-linux-amd64.tar.gz"  digest="sha256:728f9b543374b1b1733f759608e156dbe578d7b140a081084a1f4bfb4f2b3fbf"
INFO[0519] Using cache "/Users/mac/Library/Caches/lima/download/by-url-sha256/dd4613292b3a598687b9e5f97a294845661491c61e554435a27c6f6c691f8dd4/data"
INFO[0521] [hostagent] Starting QEMU (hint: to watch the boot progress, see "/Users/mac/.lima/default/serial.log")
INFO[0521] SSH Local Port: 60022
INFO[0521] [hostagent] Waiting for the essential requirement 1 of 5: "ssh"
INFO[0549] [hostagent] Waiting for the essential requirement 1 of 5: "ssh"
INFO[0559] [hostagent] Waiting for the essential requirement 1 of 5: "ssh"
INFO[0560] [hostagent] The essential requirement 1 of 5 is satisfied
INFO[0560] [hostagent] Waiting for the essential requirement 2 of 5: "user session is ready for ssh"
INFO[0574] [hostagent] Waiting for the essential requirement 2 of 5: "user session is ready for ssh"
INFO[0574] [hostagent] The essential requirement 2 of 5 is satisfied
INFO[0574] [hostagent] Waiting for the essential requirement 3 of 5: "sshfs binary to be installed"
INFO[0598] [hostagent] The essential requirement 3 of 5 is satisfied
INFO[0598] [hostagent] Waiting for the essential requirement 4 of 5: "/etc/fuse.conf (/etc/fuse3.conf) to contain \"user_allow_other\""
INFO[0601] [hostagent] The essential requirement 4 of 5 is satisfied
INFO[0601] [hostagent] Waiting for the essential requirement 5 of 5: "the guest agent to be running"
INFO[0602] [hostagent] The essential requirement 5 of 5 is satisfied
INFO[0602] [hostagent] Mounting "/Users/mac" on "/Users/mac"
INFO[0602] [hostagent] Mounting "/tmp/lima" on "/tmp/lima"
INFO[0602] [hostagent] Waiting for the optional requirement 1 of 2: "systemd must be available"
INFO[0602] [hostagent] Forwarding "/run/lima-guestagent.sock" (guest) to "/Users/mac/.lima/default/ga.sock" (host)
INFO[0602] [hostagent] The optional requirement 1 of 2 is satisfied
INFO[0602] [hostagent] Waiting for the optional requirement 2 of 2: "containerd binaries to be installed"
INFO[0602] [hostagent] Not forwarding TCP 127.0.0.53:53
INFO[0602] [hostagent] Not forwarding TCP 0.0.0.0:22
INFO[0602] [hostagent] Not forwarding TCP [::]:22
INFO[0608] [hostagent] The optional requirement 2 of 2 is satisfied
INFO[0608] [hostagent] Waiting for the final requirement 1 of 1: "boot scripts must have finished"
INFO[0620] [hostagent] The final requirement 1 of 1 is satisfied
INFO[0620] READY. Run `lima` to open the shell.
mac@13-inch-MacBook-Pro ~ %
```
Checking `ll /usr/local/libexec/cni -v` again shows the following output (the isolation binary is no longer present):
```
mac@13-inch-MacBook-Pro ~ % lima
mac@lima-default:/Users/mac$
mac@lima-default:/Users/mac$ ll /usr/local/libexec/cni -v
total 63736
drwxrwxr-x 2 root root    4096 Jun 18 16:17 ./
drwxr-xr-x 3 root root    4096 Jun 18 16:17 ../
-rwxr-xr-x 1 root root 3780654 Mar  9  2022 bandwidth*
-rwxr-xr-x 1 root root 4221977 Mar  9  2022 bridge*
-rwxr-xr-x 1 root root 9742834 Mar  9  2022 dhcp*
-rwxr-xr-x 1 root root 4345726 Mar  9  2022 firewall*
-rwxr-xr-x 1 root root 3811793 Mar  9  2022 host-device*
-rwxr-xr-x 1 root root 3241605 Mar  9  2022 host-local*
-rwxr-xr-x 1 root root 3922560 Mar  9  2022 ipvlan*
-rwxr-xr-x 1 root root 3295519 Mar  9  2022 loopback*
-rwxr-xr-x 1 root root 3959868 Mar  9  2022 macvlan*
-rwxr-xr-x 1 root root 3679140 Mar  9  2022 portmap*
-rwxr-xr-x 1 root root 4092460 Mar  9  2022 ptp*
-rwxr-xr-x 1 root root 3484284 Mar  9  2022 sbr*
-rwxr-xr-x 1 root root 2818627 Mar  9  2022 static*
-rwxr-xr-x 1 root root 3379564 Mar  9  2022 tuning*
-rwxr-xr-x 1 root root 3920827 Mar  9  2022 vlan*
-rwxr-xr-x 1 root root 3523475 Mar  9  2022 vrf*
```
Next attempt showed some progress but it still failed :(
```bash
lima nerdctl compose up 
```
Started fine but the Informix DB died on the [3rd attempt](output/docker-compose-up1.log).

---

So, I log into the Informix container to figure out what might be going on.

Was consistently getting `FATA[0014] exec failed with exit code 137` whenever I shell into the Informix container, so tried to increase VM RAM.

```bash
# lima nerdctl compose down
limactl stop default

# increase instance VM RAM, See below.
# ...

limactl start default

lima nerdctl start tc-online-db 
lima nerdctl exec -it tc-online-db bash
```
Was running out of time so had to abort the investigation.


### Allocate More RAM to `lima` VM [^1]
```bash
export LIMA_HOME=~/.lima
cat << EOF >> $LIMA_HOME/_config/override.yaml
# For more info, see https://github.com/lima-vm/lima/blob/master/examples/default.yaml
# Memory size
# 🟢 Builtin default: "4GiB"
memory: "10GiB"

EOF
```

---
[^1]: The typo in the command below seemed to have caused a crash in macOS 12.6 when I pasted the text below into iTerm2
 
```bash
cat EOF >> $LIMA_HOME/_config/override.yaml 
```