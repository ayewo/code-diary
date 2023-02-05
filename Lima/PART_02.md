# PART 02
# [PART 01](PART_01.md) | [PART 02](PART_02.md) | [PART 03](PART_03.md)
---

# January 3, 2023
Before diving in, upgrade `brew`:
```bash
brew --version
Homebrew 3.5.2

brew update

brew --version
Homebrew 3.6.17
```
Next, upgrade `lima`:
```bash
limactl stop

brew upgrade lima
==> Upgrading 1 outdated package:
lima 0.11.1 -> 0.14.2

limactl start default
```

# January 6, 2023
The path `/Users/mac/Library/Caches/lima/download/by-url-sha256` is a cache folder where `containerd` downloads are written
```bash
cd /Users/mac/Library/Caches/lima/download/by-url-sha256
for f in $(find . -name "url" -type f); do echo -n $f && echo -n "-->  " && cat $f && echo ""; done

mac@13-inch-MacBook-Pro by-url-sha256 % for f in $(find . -name "url" -type f); do echo -n $f && echo -n "-->  " && cat $f && echo ""; done

./fdbae42d5655d51afaedd0563a278bc93ea15d4f90aa8722164682cb77fff619/url-->  https://github.com/containerd/nerdctl/releases/download/v0.11.0/nerdctl-full-0.11.0-linux-amd64.tar.gz
./3304d173f1e1824e5be6cf84bf2f78825cf0db416c4c975dbb2458776942629e/url-->  https://github.com/containerd/nerdctl/releases/download/v0.11.1/nerdctl-full-0.11.1-linux-amd64.tar.gz
./62c871f6f494814b9c6555e6ac74e1e81c35ba6f12beec1a0d74c7bee5c8a41d/url-->  https://cloud-images.ubuntu.com/releases/22.04/release-20220420/ubuntu-22.04-server-cloudimg-amd64.img
./6b15519b255a45a238b7a8154cd57da120344ea388143af2821bb790af7fc587/url-->  https://cloud-images.ubuntu.com/releases/22.04/release/ubuntu-22.04-server-cloudimg-amd64.img
./e1fed960ebd29619676c7ab7535bc83f7fb2ad71739edb6fde4e17bce0b61a47/url-->  https://cloud-images.ubuntu.com/hirsute/current/hirsute-server-cloudimg-amd64.img
./dd4613292b3a598687b9e5f97a294845661491c61e554435a27c6f6c691f8dd4/url-->  https://github.com/containerd/nerdctl/releases/download/v0.21.0/nerdctl-full-0.21.0-linux-amd64.tar.gz
./dabf40bf0b785bdfa60d957219520047fd7f68de336c973b22796a9aa1dcf1f6/url-->  https://github.com/containerd/nerdctl/releases/download/v1.1.0/nerdctl-full-1.1.0-linux-amd64.tar.gz

mac@13-inch-MacBook-Pro by-url-sha256 % ls -altr
total 0
drwx------  3 mac  staff   96 Aug 19  2021 ..
drwx------  5 mac  staff  160 Aug 19  2021 fdbae42d5655d51afaedd0563a278bc93ea15d4f90aa8722164682cb77fff619
drwx------  4 mac  staff  128 Aug 19  2021 e1fed960ebd29619676c7ab7535bc83f7fb2ad71739edb6fde4e17bce0b61a47
drwx------  5 mac  staff  160 Sep  9  2021 3304d173f1e1824e5be6cf84bf2f78825cf0db416c4c975dbb2458776942629e
drwx------  5 mac  staff  160 Sep 30 10:56 dd4613292b3a598687b9e5f97a294845661491c61e554435a27c6f6c691f8dd4
drwx------  4 mac  staff  128 Oct  1 07:53 62c871f6f494814b9c6555e6ac74e1e81c35ba6f12beec1a0d74c7bee5c8a41d
drwx------  4 mac  staff  128 Oct  1 08:00 6b15519b255a45a238b7a8154cd57da120344ea388143af2821bb790af7fc587
drwx------  9 mac  staff  288 Jan  6 11:17 .
drwx------  5 mac  staff  160 Jan  6 11:25 dabf40bf0b785bdfa60d957219520047fd7f68de336c973b22796a9aa1dcf1f6
mac@13-inch-MacBook-Pro by-url-sha256 %

# it seems the two files below are identical. TODO compare SHA hashes
ll /Users/mac/Library/Caches/lima/download/by-url-sha256/6b15519b255a45a238b7a8154cd57da120344ea388143af2821bb790af7fc587 
total 1291016
-rw-r--r--  1 mac  staff  660996096 Oct  1 08:00 data
-rw-r--r--  1 mac  staff         93 Oct  1 07:53 url

ll /Users/mac/.lima/default/basedisk
-rw-r--r--  1 mac  staff    660996096 Oct  1 08:00 basedisk
```
- [ ] todo figure out a way to make `find ...` and `ls -altr` outputs match

# January 7, 2023
```bash
cd /Users/mac/Library/Caches/lima/download/by-url-sha256

for f in $(find . -type file -name "data"); do ls -l $f; done
->
-rw-r--r--  1 mac  staff  183887277 Aug 19  2021 ./fdbae42d5655d51afaedd0563a278bc93ea15d4f90aa8722164682cb77fff619/data
-rw-r--r--  1 mac  staff  181061911 Sep  9  2021 ./3304d173f1e1824e5be6cf84bf2f78825cf0db416c4c975dbb2458776942629e/data
-rw-r--r--  1 mac  staff  660996096 Oct  1 08:00 ./6b15519b255a45a238b7a8154cd57da120344ea388143af2821bb790af7fc587/data
-rw-r--r--  1 mac  staff  580321280 Aug 19  2021 ./e1fed960ebd29619676c7ab7535bc83f7fb2ad71739edb6fde4e17bce0b61a47/data
-rw-r--r--  1 mac  staff  228429692 Sep 30 10:56 ./dd4613292b3a598687b9e5f97a294845661491c61e554435a27c6f6c691f8dd4/data
-rw-r--r--  1 mac  staff  235161071 Jan  6 11:25 ./dabf40bf0b785bdfa60d957219520047fd7f68de336c973b22796a9aa1dcf1f6/data        <-- nerdctl-full-1.1.0-linux-amd64.tar.gz
```

Review the contents of `nerdctl-full-1.1.0-linux-amd64.tar.gz`:
```bash
mkdir /tmp/nuke && cd /tmp/nuke
#curl -L https://github.com/containerd/nerdctl/releases/download/v1.1.0/nerdctl-full-1.1.0-linux-amd64.tar.gz -o nerdctl-full-1.1.0-linux-amd64.tar.gz
cp /Users/mac/Library/Caches/lima/download/by-url-sha256/dabf40bf0b785bdfa60d957219520047fd7f68de336c973b22796a9aa1dcf1f6/data /tmp/nuke/nerdctl-full-1.1.0-linux-amd64.tar.gz
```

<details>
<summary><pre>tar zxvf nerdctl-full-1.1.0-linux-amd64.tar.gz</pre></summary>
<pre>
x bin/
x bin/buildctl
x bin/buildg
x bin/buildkitd
x bin/bypass4netns
x bin/bypass4netnsd
x bin/containerd
x bin/containerd-fuse-overlayfs-grpc
x bin/containerd-rootless-setuptool.sh
x bin/containerd-rootless.sh
x bin/containerd-shim-runc-v2
x bin/containerd-stargz-grpc
x bin/ctd-decoder
x bin/ctr
x bin/ctr-enc
x bin/ctr-remote
x bin/fuse-overlayfs
x bin/ipfs
x bin/nerdctl
x bin/rootlessctl
x bin/rootlesskit
x bin/runc
x bin/slirp4netns
x bin/tini
x lib/
x lib/systemd/
x lib/systemd/system/
x lib/systemd/system/buildkit.service
x lib/systemd/system/containerd.service
x lib/systemd/system/stargz-snapshotter.service
x libexec/
x libexec/cni/
x libexec/cni/bandwidth
x libexec/cni/bridge
x libexec/cni/dhcp
x libexec/cni/firewall
x libexec/cni/host-device
x libexec/cni/host-local
x libexec/cni/ipvlan
x libexec/cni/loopback
x libexec/cni/macvlan
x libexec/cni/portmap
x libexec/cni/ptp
x libexec/cni/sbr
x libexec/cni/static
x libexec/cni/tuning
x libexec/cni/vlan
x libexec/cni/vrf
x share/
x share/doc/
x share/doc/nerdctl/
x share/doc/nerdctl/README.md
x share/doc/nerdctl/docs/
x share/doc/nerdctl/docs/build.md
x share/doc/nerdctl/docs/builder-debug.md
x share/doc/nerdctl/docs/cni.md
x share/doc/nerdctl/docs/compose.md
x share/doc/nerdctl/docs/config.md
x share/doc/nerdctl/docs/cosign.md
x share/doc/nerdctl/docs/dir.md
x share/doc/nerdctl/docs/experimental.md
x share/doc/nerdctl/docs/faq.md
x share/doc/nerdctl/docs/freebsd.md
x share/doc/nerdctl/docs/gpu.md
x share/doc/nerdctl/docs/ipfs.md
x share/doc/nerdctl/docs/multi-platform.md
x share/doc/nerdctl/docs/nydus.md
x share/doc/nerdctl/docs/ocicrypt.md
x share/doc/nerdctl/docs/overlaybd.md
x share/doc/nerdctl/docs/registry.md
x share/doc/nerdctl/docs/rootless.md
x share/doc/nerdctl/docs/stargz.md
x share/doc/nerdctl-full/
x share/doc/nerdctl-full/README.md
x share/doc/nerdctl-full/SHA256SUMS

rm -rf /tmp/nuke
</pre>
</details>

---

# January 7, 2023
Start from scratch with the latest version of `lima` so I can learn why a working `docker-compose.yml` is failing with `lima`.

## 1st attempt
**Step 1**
```bash
lima nerdctl compose up

WARN[0000] default network named "bridge" does not have an internal nerdctl ID or nerdctl-managed config file, it was most likely NOT created by nerdctl
WARN[0000] build.config should be relative path, got "/Users/mac/tc/tc-online-review/local/db"
WARN[0000] build.config should be relative path, got "/Users/mac/tc/tc-online-review"
INFO[0000] Ensuring image munkyboy/fakesmtp
INFO[0000] Ensuring image tc-informix:latest
INFO[0000] Ensuring image redis:3.2.10
INFO[0000] Ensuring image tc-online-review:latest
INFO[0000] Creating container tc-online-db
INFO[0000] Creating container tc-online-review_tc-cache_1
INFO[0000] Creating container tc-online-review_tc-online-review_1
INFO[0000] Creating container tc-online-review_tc-fakesmtp_1
FATA[0000] currently StdinOpen(-i) and Tty(-t) should be same
```

**Step 2: Isolating the error**
```bash
lima nerdctl compose up tc-informix --remove-orphans
WARN[0000] default network named "bridge" does not have an internal nerdctl ID or nerdctl-managed config file, it was most likely NOT created by nerdctl
WARN[0000] build.config should be relative path, got "/Users/mac/tc/tc-online-review/local/db"
INFO[0000] Ensuring image tc-informix:latest
INFO[0000] Creating container tc-online-db
FATA[0000] currently StdinOpen(-i) and Tty(-t) should be same
```

**Step 3: view network list**
```bash
lima nerdctl network list
NETWORK ID    NAME                        FILE
              bridge                      /home/mac.linux/.config/cni/net.d/nerdctl-bridge.conflist
              tc-online-review_default    /home/mac.linux/.config/cni/net.d/nerdctl-tc-online-review_default.conflist
              host
              none
```

**Step 4: view the network config**
```bash
lima nerdctl network inspect --mode=native bridge

[
    {
        "CNI": {
            "cniVersion": "1.0.0",
            "name": "bridge",
            "nerdctlID": 0,
            "nerdctlLabels": {},
            "plugins": [
                {
                    "type": "bridge",
                    "bridge": "nerdctl0",
                    "isGateway": true,
                    "ipMasq": true,
                    "hairpinMode": true,
                    "ipam": {
                        "ranges": [
                            [
                                {
                                    "gateway": "10.4.0.1",
                                    "subnet": "10.4.0.0/24"
                                }
                            ]
                        ],
                        "routes": [
                            {
                                "dst": "0.0.0.0/0"
                            }
                        ],
                        "type": "host-local"
                    }
                },
                {
                    "type": "portmap",
                    "capabilities": {
                        "portMappings": true
                    }
                },
                {
                    "type": "firewall",
                    "ingressPolicy": "same-bridge"
                },
                {
                    "type": "tuning"
                }
            ]
        },
        "NerdctlID": null,
        "File": "/home/mac.linux/.config/cni/net.d/nerdctl-bridge.conflist"
    }
]
```

**Step 5: there are small differences between the default `bridge` network and `tc-online-review_default`**
```bash
lima nerdctl network inspect --mode=native tc-online-review_default

[
    {
        "CNI": {
            "cniVersion": "1.0.0",
            "name": "tc-online-review_default",
            "nerdctlID": 1,
            "nerdctlLabels": {
                "com.docker.compose.network": "default",
                "com.docker.compose.project": "tc-online-review"
            },
            "plugins": [
                {
                    "type": "bridge",
                    "bridge": "nerdctl1",
                    "isGateway": true,
                    "ipMasq": true,
                    "hairpinMode": true,
                    "ipam": {
                        "ranges": [
                            [
                                {
                                    "gateway": "10.4.1.1",
                                    "subnet": "10.4.1.0/24"
                                }
                            ]
                        ],
                        "routes": [
                            {
                                "dst": "0.0.0.0/0"
                            }
                        ],
                        "type": "host-local"
                    }
                },
                {
                    "type": "portmap",
                    "capabilities": {
                        "portMappings": true
                    }
                },
                {
                    "type": "firewall",
                    "ingressPolicy": "same-bridge"
                },
                {
                    "type": "tuning"
                }
            ]
        },
        "NerdctlID": null,
        "File": "/home/mac.linux/.config/cni/net.d/nerdctl-tc-online-review_default.conflist"
    }
]
```
**Step 6: check the creation date of the network config**
```bash
lima ls -l /home/mac.linux/.config/cni/net.d/
total 8
-rw-r--r-- 1 mac mac 754 Oct  1 07:11 nerdctl-bridge.conflist
-rw-r--r-- 1 mac mac 873 Oct  1 09:19 nerdctl-tc-online-review_default.conflist
```

**Step 7: re-create a fresh `lima` VM and track the files that are touched using `watchman` (from Facebook)**
```bash
brew install watchman
==> Fetching dependencies for watchman: cmake, cpptoml, googletest, libssh2, rust, icu4c, boost, double-conversion, fmt, gflags, glog, folly, edencommon, libsodium, fizz, wangle, fbthrift and fb303
...
==> Installing dependencies for watchman: cmake, cpptoml, googletest, libssh2, rust, icu4c, boost, double-conversion, fmt, gflags, glog, folly, edencommon, libsodium, fizz, wangle, fbthrift and fb303
==> Installing watchman dependency: cmake
==> Pouring cmake--3.25.1.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/cmake/3.25.1: 3,167 files, 49.1MB
==> Installing watchman dependency: cpptoml
==> Pouring cpptoml--0.1.1_1.all.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/cpptoml/0.1.1_1: 8 files, 112.7KB
==> Installing watchman dependency: googletest
==> Pouring googletest--1.12.1_1.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/googletest/1.12.1_1: 75 files, 2.1MB
==> Installing watchman dependency: libssh2
==> Pouring libssh2--1.10.0.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/libssh2/1.10.0: 184 files, 999.9KB
==> Installing watchman dependency: rust
==> Pouring rust--1.66.0.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/rust/1.66.0: 35,977 files, 876.8MB
==> Installing watchman dependency: icu4c
==> Pouring icu4c--71.1.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/icu4c/71.1: 262 files, 76.2MB
==> Installing watchman dependency: boost
==> Pouring boost--1.80.0.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/boost/1.80.0: 15,568 files, 472MB
==> Installing watchman dependency: double-conversion
==> Pouring double-conversion--3.2.1.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/double-conversion/3.2.1: 26 files, 242.3KB
==> Installing watchman dependency: fmt
==> Pouring fmt--9.1.0.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/fmt/9.1.0: 27 files, 1MB
==> Installing watchman dependency: gflags
==> Pouring gflags--2.2.2.monterey.bottle.2.tar.gz
🍺  /Users/mac/homebrew/Cellar/gflags/2.2.2: 26 files, 712.9KB
==> Installing watchman dependency: glog
==> Pouring glog--0.6.0.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/glog/0.6.0: 22 files, 315.8KB
==> Installing watchman dependency: folly
==> Pouring folly--2023.01.02.00.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/folly/2023.01.02.00: 841 files, 24.3MB
==> Installing watchman dependency: edencommon
==> Pouring edencommon--2023.01.02.00.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/edencommon/2023.01.02.00: 15 files, 303.2KB
==> Installing watchman dependency: libsodium
==> Pouring libsodium--1.0.18_1.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/libsodium/1.0.18_1: 73 files, 1MB
==> Installing watchman dependency: fizz
==> Pouring fizz--2023.01.02.00.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/fizz/2023.01.02.00: 184 files, 3.8MB
==> Installing watchman dependency: wangle
==> Pouring wangle--2023.01.02.00.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/wangle/2023.01.02.00: 117 files, 3.2MB
==> Installing watchman dependency: fbthrift
==> Pouring fbthrift--2023.01.02.00.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/fbthrift/2023.01.02.00: 759 files, 21.2MB
==> Installing watchman dependency: fb303
==> Pouring fb303--2023.01.02.00.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/fb303/2023.01.02.00: 57 files, 3.6MB
==> Installing watchman
==> cmake -S . -B build -DENABLE_EDEN_SUPPORT=ON -DWATCHMAN_VERSION_OVERRIDE=2023.01.02.00 -DWATCHMAN_BUILDINFO_OVERRIDE=Homebrew -DWATCHMAN_STATE_DIR=/Users/mac/homebrew/var/run/watchman
==> cmake --build build
==> cmake --install build
🍺  /Users/mac/homebrew/Cellar/watchman/2023.01.02.00: 28 files, 14.4MB, built in 10 minutes 17 seconds
==> Running `brew cleanup watchman`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).


watchman version
version: 2023.01.02.00
```

**Set up `watchman`**
```bash
watchman watch ~/.lima/default
watchman -- trigger ~/.lima/default log '*.*' -- echo
```
Apparently, `watchman` is not simple to use so aborting.


**Step 8: install `fswatch` as `watchman` (from Facebook) is too enterprisey**
```bash
brew install fswatch
==> Fetching fswatch
==> Downloading https://ghcr.io/v2/homebrew/core/fswatch/manifests/1.17.1
######################################################################## 100.0%
==> Downloading https://ghcr.io/v2/homebrew/core/fswatch/blobs/sha256:6c57d2ea9ff9e425069580bba25c74f5890f454b807f4a94810271909d47283e
==> Downloading from https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:6c57d2ea9ff9e425069580bba25c74f5890f454b807f4a94810271909d47283e?se=2023-01-07T10%3A45%3A00Z&sig=ejkGo1qalLgeYqPwHzo5TFjVsj
######################################################################## 100.0%
==> Pouring fswatch--1.17.1.monterey.bottle.tar.gz
🍺  /Users/mac/homebrew/Cellar/fswatch/1.17.1: 52 files, 1.2MB
==> Running `brew cleanup fswatch`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
```

**Set up `fswatch`**
```bash
# fswatch -o ~/.lima/default | xargs -n1 -I{} echo {}
fswatch -0 -tvr ~/homebrew/Cellar/qemu/ ~/.lima/default ~/Library/Caches/lima/download/by-url-sha256 | xargs -0 -n1 -I {} echo {}
->
register_signal_handlers: SIGTERM handler registered.
register_signal_handlers: SIGABRT handler registered.
register_signal_handlers: SIGINT handler registered.
start_monitor: Adding path: /Users/mac/homebrew/Cellar/qemu
start_monitor: Adding path: /Users/mac/.lima/default
start_monitor: Adding path: /Users/mac/Library/Caches/lima/download/by-url-sha256
create_stream: Creating FSEvent stream...
run: Starting event stream...
notify_events: Notifying events #: 2.
Sat Jan  7 15:18:55 2023 /Users/mac/.lima/default/ga.sock
Sat Jan  7 15:18:55 2023 /Users/mac/.lima/default/ssh.sock
```

**Small detour to using `lsof` for the same purpose of tracking open file handles**
```bash
# lsof -i :25; lsof -i tcp; lsof -i udp;
# list open files for processes named "lima", "limactl" & "qemu"; defaults to 15 seconds of polling
lsof -c lima -c limactl -c qemu -r

# 5 seconds of polling
lsof -c lima -c limactl -c qemu -r 5
```
---
### Egress Network Traffic logged by `Lulu` from `lima` and `qemu`
```bash
/Users/mac/homebrew/Cellar/lima/0.14.2/bin/limactl hostagent --pidfile /Users/mac/.lima/default/ha.pid --socket /Users/mac/.lima/default/ha.sock --nerdctl-archive /Users/mac/Library/Caches/lima/download/by-url-sha256/dabf40bf0b785bdfa60d957219520047fd7f68de336c973b22796a9aa1dcf1f6/data default 

/Users/mac/homebrew/Cellar/qemu/7.2.0/bin/qemu-system-x86_64 -m 10240 -cpu host -machine q35,accel=hvf -smp 4,sockets=1,cores=4,threads=1 -drive if=pflash,format=raw,readonly=on,file=/Users/mac/homebrew/share/qemu/edk2-x86_64-code.fd -boot order=c,splash-time=0,menu=on -drive file=/Users/mac/.lima/default/diffdisk,if=virtio,discard=on -cdrom /Users/mac/.lima/default/cidata.iso -netdev user,id=net0,net=192.168.5.0/24,dhcpstart=192.168.5.15,hostfwd=tcp:127.0.0.1:60022-:22 -device virtio-net-pci,netdev=net0,mac=52:55:55:72:c3:fe -device virtio-rng-pci -display none -device virtio-vga -device virtio-keyboard-pci -device virtio-mouse-pci -device qemu-xhci,id=usb-bus -parallel none -chardev socket,id=char-serial,path=/Users/mac/.lima/default/serial.sock,server=on,wait=off,logfile=/Users/mac/.lima/default/serial.log -serial chardev:char-serial -chardev socket,id=char-qmp,path=/Users/mac/.lima/default/qmp.sock,server=on,wait=off -qmp chardev:char-qmp -name lima-default -pidfile /Users/mac/.lima/default/qemu.pid 

/Users/mac/homebrew/Cellar/qemu/7.2.0/bin/qemu-system-x86_64 -m 10240 -cpu host -machine q35,accel=hvf -smp 4,sockets=1,cores=4,threads=1 -drive if=pflash,format=raw,readonly=on,file=/Users/mac/homebrew/share/qemu/edk2-x86_64-code.fd -boot order=c,splash-time=0,menu=on -drive file=/Users/mac/.lima/default/diffdisk,if=virtio,discard=on -cdrom /Users/mac/.lima/default/cidata.iso -netdev user,id=net0,net=192.168.5.0/24,dhcpstart=192.168.5.15,hostfwd=tcp:127.0.0.1:60022-:22 -device virtio-net-pci,netdev=net0,mac=52:55:55:72:c3:fe -device virtio-rng-pci -display none -device virtio-vga -device virtio-keyboard-pci -device virtio-mouse-pci -device qemu-xhci,id=usb-bus -parallel none -chardev socket,id=char-serial,path=/Users/mac/.lima/default/serial.sock,server=on,wait=off,logfile=/Users/mac/.lima/default/serial.log -serial chardev:char-serial -chardev socket,id=char-qmp,path=/Users/mac/.lima/default/qmp.sock,server=on,wait=off -qmp chardev:char-qmp -name lima-default -pidfile /Users/mac/.lima/default/qemu.pid 

/Users/mac/homebrew/Cellar/qemu/7.2.0/bin/qemu-system-x86_64 -m 10240 -cpu host -machine q35,accel=hvf -smp 4,sockets=1,cores=4,threads=1 -drive if=pflash,format=raw,readonly=on,file=/Users/mac/homebrew/share/qemu/edk2-x86_64-code.fd -boot order=c,splash-time=0,menu=on -drive file=/Users/mac/.lima/default/diffdisk,if=virtio,discard=on -cdrom /Users/mac/.lima/default/cidata.iso -netdev user,id=net0,net=192.168.5.0/24,dhcpstart=192.168.5.15,hostfwd=tcp:127.0.0.1:60022-:22 -device virtio-net-pci,netdev=net0,mac=52:55:55:72:c3:fe -device virtio-rng-pci -display none -device virtio-vga -device virtio-keyboard-pci -device virtio-mouse-pci -device qemu-xhci,id=usb-bus -parallel none -chardev socket,id=char-serial,path=/Users/mac/.lima/default/serial.sock,server=on,wait=off,logfile=/Users/mac/.lima/default/serial.log -serial chardev:char-serial -chardev socket,id=char-qmp,path=/Users/mac/.lima/default/qmp.sock,server=on,wait=off -qmp chardev:char-qmp -name lima-default -pidfile /Users/mac/.lima/default/qemu.pid 
```

**Explore inside the `lima` VM to get a sense of how networking is done:**
```bash
# Ctrl+T 
lima

ip address show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:55:55:72:c3:fe brd ff:ff:ff:ff:ff:ff
    inet 192.168.5.15/24 metric 100 brd 192.168.5.255 scope global dynamic eth0
       valid_lft 85755sec preferred_lft 85755sec
    inet6 fec0::5055:55ff:fe72:c3fe/64 scope site dynamic mngtmpaddr noprefixroute
       valid_lft 86099sec preferred_lft 14099sec
    inet6 fe80::5055:55ff:fe72:c3fe/64 scope link
       valid_lft forever preferred_lft forever


ip route show
default via 192.168.5.2 dev eth0 proto dhcp src 192.168.5.15 metric 100
192.168.5.0/24 dev eth0 proto kernel scope link src 192.168.5.15 metric 100
192.168.5.2 dev eth0 proto dhcp scope link src 192.168.5.15 metric 100
192.168.5.3 dev eth0 proto dhcp scope link src 192.168.5.15 metric 100
```

**List the file paths of the config used by `nerdctl`:**
```bash
nerdctl help
->
/home/mac.linux/.config/nerdctl/nerdctl.toml
/home/mac.linux/.config/cni/net.d
/home/mac.linux/.local/share/nerdctl/1935db59/volumes/default/
```

**List the existing networks**
```bash
nerdctl network list
->
NETWORK ID    NAME                        FILE
              bridge                      /home/mac.linux/.config/cni/net.d/nerdctl-bridge.conflist
              tc-online-review_default    /home/mac.linux/.config/cni/net.d/nerdctl-tc-online-review_default.conflist
              host
              none
```

**Remove the existing `tc-online-review_default` network bridge created when `lima` failed to start the `docker-compose.yml`:**
```bash
nerdctl network rm tc-online-review_default
```

**Re-attempt to launch individual containers specified in `docker-compose.yml` using `lima`:**
```bash
lima nerdctl compose up tc-informix --remove-orphans  # same error
lima nerdctl compose up tc-cache --remove-orphans # works
```
The error is still the same as before. So it is something deeper than just a misconfigured container network.

With open file handles tracking, I noticed the following file handles were open in `lsof` when I ran `lima nerdctl compose up tc-cache?`:
```bash

  /run/containerd/io.containerd.runtime.v2.task/default/8bed4cec6b019de5d3c7ae6b28d9f22f6a871b592f8cba9ae0aa4babba9f7df6   <--- inaccessible
  export PREFIX=/home/mac.linux/.local/share/nerdctl/1935db59/containers/default/8bed4cec6b019de5d3c7ae6b28d9f22f6a871b592f8cba9ae0aa4babba9f7df6
  $PREFIX/oci-hook.createRuntime.log <-- WARNING to stdout is written here
  $PREFIX/8bed4cec6b019de5d3c7ae6b28d9f22f6a871b592f8cba9ae0aa4babba9f7df6-json.log  <-- stdout is written here
```

**List container images:**
```bash
nerdctl images
REPOSITORY           TAG       IMAGE ID        CREATED         PLATFORM       SIZE         BLOB SIZE
redis                3.2.10    6da3e8bc8e93    3 months ago    linux/amd64    106.4 MiB    35.1 MiB
tc-informix          latest    59b18afd1ad0    3 months ago    linux/amd64    2.9 GiB      1.2 GiB
tc-online-review     latest    b9bfe1e287b2    3 months ago    linux/amd64    658.8 MiB    423.7 MiB
munkyboy/fakesmtp    latest    190e5ae5a056    3 months ago    linux/amd64    351.2 MiB    143.0 MiB
```

**Export container images locally to save on network bandwidth:**
```bash
nerdctl save redis:3.2.10 -o tc-redis.tar.gz
nerdctl save tc-informix:latest -o tc-informix.tar.gz
nerdctl save tc-online-review:latest -o tc-online-review.tar.gz
nerdctl save munkyboy/fakesmtp:latest -o tc-fakesmtp.tar.gz

# /tmp/lima is shared between lima and macOS but since the VM will be recreated, safer to move them to a different path
mv tc-* /Users/mac/tc/lima
```
Following the discussion in [Prevent double-creation of nerdctl default network. #1538](https://github.com/containerd/nerdctl/pull/1538) best path forward is to recreate the lima VM so I decided to abort the investigation.

**Re-creating a fresh `lima` VM from scratch**
```bash
limactl rm -f $(limactl ls -q)
limactl start default

? Creating an instance "default" Proceed with the current configuration
INFO[0087] Attempting to download the image from "https://cloud-images.ubuntu.com/releases/22.10/release-20221201/ubuntu-22.10-server-cloudimg-amd64.img"  digest="sha256:4228fae635160ee2eeebda7b3f466e99729121958c125c6fbefe79178355d09b"
682.00 MiB / 682.00 MiB [---------------------------------] 100.00% 205.04 KiB/s
INFO[3513] Downloaded the image from "https://cloud-images.ubuntu.com/releases/22.10/release-20221201/ubuntu-22.10-server-cloudimg-amd64.img"
INFO[3516] Attempting to download the nerdctl archive from "https://github.com/containerd/nerdctl/releases/download/v1.1.0/nerdctl-full-1.1.0-linux-amd64.tar.gz"  digest="sha256:5440c7b3af63df2ad2c98e185e06a27b4a21eea334b05408e84f8502251d9459"
INFO[3516] Using cache "/Users/mac/Library/Caches/lima/download/by-url-sha256/dabf40bf0b785bdfa60d957219520047fd7f68de336c973b22796a9aa1dcf1f6/data"
INFO[3518] [hostagent] Starting QEMU (hint: to watch the boot progress, see "/Users/mac/.lima/default/serial.log")
INFO[3518] SSH Local Port: 60022
INFO[3518] [hostagent] Waiting for the essential requirement 1 of 5: "ssh"
INFO[3546] [hostagent] Waiting for the essential requirement 1 of 5: "ssh"
INFO[3559] [hostagent] The essential requirement 1 of 5 is satisfied
INFO[3559] [hostagent] Waiting for the essential requirement 2 of 5: "user session is ready for ssh"
INFO[3580] [hostagent] Waiting for the essential requirement 2 of 5: "user session is ready for ssh"
INFO[3580] [hostagent] The essential requirement 2 of 5 is satisfied
INFO[3580] [hostagent] Waiting for the essential requirement 3 of 5: "sshfs binary to be installed"
INFO[3620] [hostagent] Waiting for the essential requirement 3 of 5: "sshfs binary to be installed"
INFO[3660] [hostagent] Waiting for the essential requirement 3 of 5: "sshfs binary to be installed"
INFO[3666] [hostagent] The essential requirement 3 of 5 is satisfied
INFO[3666] [hostagent] Waiting for the essential requirement 4 of 5: "/etc/fuse.conf (/etc/fuse3.conf) to contain \"user_allow_other\""
INFO[3670] [hostagent] The essential requirement 4 of 5 is satisfied
INFO[3670] [hostagent] Waiting for the essential requirement 5 of 5: "the guest agent to be running"
INFO[3670] [hostagent] The essential requirement 5 of 5 is satisfied
INFO[3670] [hostagent] Mounting "/Users/mac" on "/Users/mac"
INFO[3670] [hostagent] Mounting "/tmp/lima" on "/tmp/lima"
INFO[3670] [hostagent] Waiting for the optional requirement 1 of 2: "systemd must be available"
INFO[3670] [hostagent] Forwarding "/run/lima-guestagent.sock" (guest) to "/Users/mac/.lima/default/ga.sock" (host)
INFO[3670] [hostagent] The optional requirement 1 of 2 is satisfied
INFO[3670] [hostagent] Waiting for the optional requirement 2 of 2: "containerd binaries to be installed"
INFO[3670] [hostagent] Not forwarding TCP 127.0.0.54:53
INFO[3670] [hostagent] Not forwarding TCP 127.0.0.53:53
INFO[3670] [hostagent] Not forwarding TCP [::]:22
INFO[3683] [hostagent] The optional requirement 2 of 2 is satisfied
INFO[3683] [hostagent] Waiting for the final requirement 1 of 1: "boot scripts must have finished"
INFO[3695] [hostagent] The final requirement 1 of 1 is satisfied
INFO[3695] READY. Run `lima` to open the shell.
```

---

# January 8, 2023
**Start only the `tc-cache` container**
```bash
cd /Users/mac/tc/tc-online-review
lima nerdctl compose up tc-cache --remove-orphans

INFO[0000] Creating network tc-online-review_default
INFO[0000] Ensuring image redis:3.2.10
docker.io/library/redis:3.2.10:                                                   resolved       |++++++++++++++++++++++++++++++++++++++|
index-sha256:6da3e8bc8e937890cc490f5ce052fee25ef1ce8c80caa5dad5dc59334e8435f5:    done           |++++++++++++++++++++++++++++++++++++++|
manifest-sha256:07f04053229d5bd6c4d1fbb72b6ca52dee50b3fdaf862c0a2859260a6b00776a: done           |++++++++++++++++++++++++++++++++++++++|
config-sha256:ab80677cd595ebda80d957a2de8e1bd946de9d875fa5044188f9a5a6b4fbce25:   done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:67707da4b500318e434eb7dafbb1d88273d03c034319f680515a3841c56937d1:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:f4a0d1212c38ec976de047e36c9b0f19d8427333c4db7d45d403f5839ec1c8fb:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:065132d9f7059b525640520932c776949f4d0d744b65429f1026f3d4d9b3615a:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:be9835c278527bc23a93cf0cbfae60655ad5cf9f26806935a1bd0da57d14c751:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:05654b1571f054bef9f412fbd6853b43f7256bdf25b93a88160cf8731579d1e5:    done           |++++++++++++++++++++++++++++++++++++++|
layer-sha256:86c6d00dffa1c5a63076ac4162daba9a323e5c87f2d9769ffd04750989bc0e3c:    done           |++++++++++++++++++++++++++++++++++++++|
elapsed: 115.8s                                                                   total:  35.1 M (310.8 KiB/s)
INFO[0116] Creating container tc-online-review_tc-cache_1
INFO[0117] Attaching to logs
tc-cache_1 |1:C 08 Jan 10:43:24.288 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
tc-cache_1 |1:M 08 Jan 10:43:24.289 # You requested maxclients of 10000 requiring at least 10032 max file descriptors.
tc-cache_1 |1:M 08 Jan 10:43:24.289 # Server can't set maximum open files to 10032 because of OS error: Operation not permitted.
tc-cache_1 |1:M 08 Jan 10:43:24.289 # Current maximum open files is 1024. maxclients has been reduced to 992 to compensate for low ulimit. If you need higher maxclients increase 'ulimit -n'.
tc-cache_1 |                _._
tc-cache_1 |           _.-``__ ''-._
tc-cache_1 |      _.-``    `.  `_.  ''-._           Redis 3.2.10 (00000000/0) 64 bit
tc-cache_1 |  .-`` .-```.  ```\/    _.,_ ''-._
tc-cache_1 | (    '      ,       .-`  | `,    )     Running in standalone mode
tc-cache_1 | |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
tc-cache_1 | |    `-._   `._    /     _.-'    |     PID: 1
tc-cache_1 |  `-._    `-._  `-./  _.-'    _.-'
tc-cache_1 | |`-._`-._    `-.__.-'    _.-'_.-'|
tc-cache_1 | |    `-._`-._        _.-'_.-'    |           http://redis.io
tc-cache_1 |  `-._    `-._`-.__.-'_.-'    _.-'
tc-cache_1 | |`-._`-._    `-.__.-'    _.-'_.-'|
tc-cache_1 | |    `-._`-._        _.-'_.-'    |
tc-cache_1 |  `-._    `-._`-.__.-'_.-'    _.-'
tc-cache_1 |      `-._    `-.__.-'    _.-'
tc-cache_1 |          `-._        _.-'
tc-cache_1 |              `-.__.-'
tc-cache_1 |
tc-cache_1 |1:M 08 Jan 10:43:24.291 # Server started, Redis version 3.2.10
tc-cache_1 |1:M 08 Jan 10:43:24.291 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
tc-cache_1 |1:M 08 Jan 10:43:24.291 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
tc-cache_1 |1:M 08 Jan 10:43:24.291 * The server is now ready to accept connections on port 6379
```


**Then look around inside the `lima` VM**
```bash
# Ctrl+T
lima

nerdctl network ls
NETWORK ID      NAME                        FILE
17f29b073143    bridge                      /home/mac.linux/.config/cni/net.d/nerdctl-bridge.conflist
133034ba36d8    tc-online-review_default    /home/mac.linux/.config/cni/net.d/nerdctl-tc-online-review_default.conflist
                host
                none
```

**Notice the default network config file `bridge` and `tc-online-review_default` are nearly identical unlike in PART 01**
```bash
diff /home/mac.linux/.config/cni/net.d/nerdctl-bridge.conflist /home/mac.linux/.config/cni/net.d/nerdctl-tc-online-review_default.conflist
3,4c3,4
<   "name": "bridge",
<   "nerdctlID": "17f29b073143d8cd97b5bbe492bdeffec1c5fee55cc1fe2112c8b9335f8b6121",
---
>   "name": "tc-online-review_default",
>   "nerdctlID": "133034ba36d8578d562c02cd75942ef2a9db51f02846a11e5fcd868b9179d3e4",
6c6,7
<     "nerdctl/default-network": "true"
---
>     "com.docker.compose.network": "default",
>     "com.docker.compose.project": "tc-online-review"
11c12
<       "bridge": "nerdctl0",
---
>       "bridge": "br-133034ba36d8",
19,20c20,21
<               "gateway": "10.4.0.1",
<               "subnet": "10.4.0.0/24"
---
>               "gateway": "10.4.1.1",
>               "subnet": "10.4.1.0/24"
```
**View process activity inside the `lima` VM**
```bash
ps auxf

mac         3877  0.0  0.0 720776  9232 ?        Sl   10:43   0:00  \_ /usr/local/bin/containerd-shim-runc-v2 -namespace default -id e62957b1ed1556aa409450e30d2da7c90beb2d40a463289ea5de7c7b6c348807 -address /
mac         3886  0.0  0.2 739676 22684 ?        Sl   10:43   0:00      \_ /usr/local/bin/nerdctl _NERDCTL_INTERNAL_LOGGING /home/mac.linux/.local/share/nerdctl/1935db59
100998      3908  1.3  0.0  33324  3568 ?        Ssl  10:43   0:10      \_ redis-server *:6379
root        2334  1.5  0.0 718492  9604 ?        Ssl  09:24   1:23 /usr/local/bin/lima-guestagent daemon
root        2507  0.0  0.0  14848  8368 ?        Ss   09:24   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root        2508  0.0  0.1  17380 10904 ?        Ss   09:24   0:00  \_ sshd: mac [priv]
mac         2557  0.0  0.0  17772  7280 ?        S    09:24   0:01      \_ sshd: mac@pts/0,pts/1
mac         2900  0.0  0.0 229168  3008 ?        Ssl  09:26   0:00          \_ sshfs :/Users/mac /Users/mac -o slave -o ro -o allow_other
mac         2911  0.0  0.0 229168  3052 ?        Ssl  09:26   0:00          \_ sshfs :/tmp/lima /tmp/lima -o slave -o allow_other
mac         3834  1.9  0.3 740444 32712 pts/0    Ssl+ 10:41   0:18          \_ nerdctl compose up tc-cache --remove-orphans
mac         4029  0.0  0.2 739676 25788 pts/0    Sl+  10:43   0:00          |   \_ /usr/local/bin/nerdctl logs -f e62957b1ed1556aa409450e30d2da7c90beb2d40a463289ea5de7c7b6c348807
mac         4043  0.0  0.0   6068  1016 pts/0    S+   10:43   0:00          |       \_ tail -n +0 -f /home/mac.linux/.local/share/nerdctl/1935db59/containers/default/e62957b1ed1556aa409450e30d2da7c90beb2d40a4
mac         4068  0.0  0.0   9284  5616 pts/1    Ss   10:49   0:00          \_ /bin/bash --login
mac         4129  0.0  0.0  11808  4164 pts/1    R+   10:56   0:00              \_ ps auxf
```

**Install midnight commander `mc` to explore the filesystem**
```bash
sudo apt install mc
cd /home/mac.linux/.local/share/nerdctl/1935db59 && mc
cd /home/mac.linux/.local/share/containerd
```

**Watch for filesystem activity inside the `lima` VM**
```bash
## Ctrl+T
lsof -c nerdctl -c containerd -r 2 > /tmp/lima/1.log && tail -f /tmp/lima/1.log
```

**Restore the larger container images**
```bash
## Ctrl+T
## Restore the remaining container images
cd /Users/mac/tc/lima
# tc-cache (redis) had been done above
lima nerdctl load -i tc-informix.tar.gz
lima nerdctl load -i tc-online-review.tar.gz
lima nerdctl load -i tc-fakesmtp.tar.gz


unpacking docker.io/library/tc-informix:latest (sha256:59b18afd1ad062c4327b14edebed5934c3e3e426364ea6e90693497b2c39d4ec)...
Loaded image: docker.io/library/tc-informix:latest

unpacking docker.io/library/tc-online-review:latest (sha256:b9bfe1e287b2df8e9dabd6c8b70bb3067f2c6df3af83867ad61a29e8d1716751)...
Loaded image: docker.io/library/tc-online-review:latest

unpacking docker.io/munkyboy/fakesmtp:latest (sha256:190e5ae5a056daae887ae361b0d51163e6660b586ec9d5896c973c5a080ea62d)...
Loaded image: docker.io/munkyboy/fakesmtp:latest
```
---

## An Inscrutable Error Message
After a importing the container images into the recreated `qemu` VM instance and attempting to launch the `docker-compose.yml` file, I got blocked by the following inscrutable error:
> FATA[0001] currently StdinOpen(-i) and Tty(-t) should be same

Google turned up unrelated results for ["currently StdinOpen(-i) and Tty(-t) should be same"](https://www.google.com/search?rls=en&q=currently+StdinOpen(-i)+and+Tty(-t)+should+be+same&ie=UTF-8&oe=UTF-8) perhaps because too few people have encountered the error. The `lima` project is still quite small compared to `docker` but most of the results pointed to `docker`.

A search for the string ["currently StdinOpen(-i) and Tty(-t) should be same"](https://github.com/lima-vm/lima/search?q=currently+StdinOpen%28-i%29+and+Tty%28-t%29+should+be+same&type=commits) on the [`lima`](https://github.com/lima-vm/lima) repo yielded no results. A simpler search for the string ["Creating container"](https://github.com/lima-vm/lima/search?q=Creating+container&type=code) yielded irrelevant results but pointed me into the direction I should be looking.

## GitHub's Search is too Basic 
`lima` uses `nerdctl` internally so I visited the [`nerdctl`](https://github.com/containerd/nerdctl) project on Github but searching for ["currently StdinOpen(-i) and Tty(-t) should be same"](https://github.com/containerd/nerdctl/search?q=currently+StdinOpen%28-i%29+and+Tty%28-t%29+should+be+same) from there directly should have returned a [**direct match**](https://github.com/containerd/nerdctl/blob/2735dfc350a25f1857df8ca672723591a4cb4424/pkg/composer/up_service.go#L161) but gave no results. 
So, I used a simpler search string from the `nerdctl` output ["Creating container"](https://github.com/containerd/nerdctl/search?q=Creating+container&type=) which led to a multiple [matches](https://github.com/containerd/nerdctl/blob/2735dfc350a25f1857df8ca672723591a4cb4424/pkg/composer/up_service.go#L131-L133) in the same file.


## Enable Verbose Output from `lima`
I needed to see what arguments `nerdctl` was using to start up the `docker-compose.yml` for the project, so was looking for a way to turn on the flag for debug output or higher verbosity.

After much [fumbling](https://github.com/containerd/nerdctl/blob/2735dfc350a25f1857df8ca672723591a4cb4424/pkg/composer/composer.go#L48) around, I returned to review the `nerdctl` docs and eventually landed on the [config](https://github.com/containerd/nerdctl/blob/c3e6ceb0de52e681f1217d6024af97bedece801d/docs/config.md#properties) which suggested `--debug-full` as the correct CLI flag. 

---

## Found the ISSUE by just comparing the CLI args: `-d` vs `-tty` with debug enabled!
```bash
lima nerdctl compose up --debug-full
->
...
INFO[0000] Creating container tc-online-review_tc-fakesmtp_1
DEBU[0000] Running [/usr/local/bin/nerdctl --debug-full=true run --cidfile=/tmp/compose-1093248909/cid -l=com.docker.compose.project=tc-online-review -l=com.docker.compose.service=tc-fakesmtp -d --name=tc-online-review_tc-fakesmtp_1 --pull=never --hostname=tc-fakesmtp --net=tc-online-review_default -p=1025:25/tcp --restart=no munkyboy/fakesmtp]
INFO[0000] Creating container tc-online-db

DEBU[0000] Running [/usr/local/bin/nerdctl --debug-full=true run --cidfile=/tmp/compose-2420943183/cid -l=com.docker.compose.project=tc-online-review -l=com.docker.compose.service=tc-informix --name=tc-online-db --pull=never -e=LICENSE=accept --hostname=informix.cloud.tc.com --net=tc-online-review_default --platform=linux/amd64 -p=2021:2021/tcp -p=2022:2022/tcp -p=27017:27017/tcp -p=27018:27018/tcp -p=27883:27883/tcp --restart=no --tty tc-informix:latest]

INFO[0000] Creating container tc-online-review_tc-online-review_1

INFO[0000] Creating container tc-online-review_tc-cache_1
DEBU[0000] Running [/usr/local/bin/nerdctl --debug-full=true run --cidfile=/tmp/compose-4242740975/cid -l=com.docker.compose.project=tc-online-review -l=com.docker.compose.service=tc-cache -d --name=tc-online-review_tc-cache_1 --pull=never --hostname=tc-cache --net=tc-online-review_default -p=6379:6379/tcp --restart=no redis:3.2.10]

DEBU[0000] Running [/usr/local/bin/nerdctl --debug-full=true run --cidfile=/tmp/compose-3042038603/cid -l=com.docker.compose.project=tc-online-review -l=com.docker.compose.service=tc-online-review -d --name=tc-online-review_tc-online-review_1 --pull=never -e=JAVA_OPTS= -Xms1G -Xmx1G -XX:MaxPermSize=256M -server -e=TZ=America/Indiana/Indianapolis --hostname=tc-online-review --net=tc-online-review_default -p=8080:8080/tcp -p=9990:9990/tcp --restart=no tc-online-review:latest]
FATA[0001] currently StdinOpen(-i) and Tty(-t) should be same
```

## Removing the `tty` flag fixed it!
The documentation for `nerdctl` clearly says that *WIP: currently -t conflicts with -d* so use of `-t, --tty` and `-d, --detach` simulatenously is [unsupported](https://github.com/containerd/nerdctl/blob/8c9b252a852bd5adeb5ddcd0ed5fd969c21cbca2/docs/command-reference.md#container-management).

I didn't want to edit `docker-compose.yml` which is under version control so instead I: `cp docker-compose.yml compose.yml` and edited `compose.yml` directly because the [compose spec rules](https://github.com/compose-spec/compose-spec/blob/7d5c7c88676ac240b6b037f868c04b33f738d82a/spec.md#compose-file) give precedence to `compose` files over `docker-compose` files, if both are present.

```bash
lima nerdctl compose up --debug-full
```
The command above generated the following [output](output/lima-compose-up.log) although the `tc-informix` container eventually exited after running setup scripts.

