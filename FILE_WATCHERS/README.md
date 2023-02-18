# FILE WATCHERS

Outside of `lsof`, these are the following file system monitors/watchers that I am aware of:
* [`watchman`](https://github.com/facebook/watchman) -- [too enterprisey for my needs so I tried `fswatch`](https://github.com/ayewo/code-diary/blob/2aff5ee44386882584a9fa2887ef4eb27ef98782/Lima/PART_02.md#step-8-install-fswatch-as-watchman-from-facebook-is-too-enterprisey)
* [`fswatch`](https://github.com/emcrisostomo/fswatch) -- no opinion yet
* [`sloth`](https://github.com/sveinbjornt/Sloth) -- GUI via HN [^1]
* [`fs_usage`](https://opensource.apple.com/source/system_cmds/system_cmds-496/fs_usage.tproj/fs_usage.c.auto.html) -- built into macOS [^1]
  * `rwsnoop` -- `dtrace`-based requires SIP (system integrity protection) to be disabled [^1]
  * `iotop` -- another `dtrace`-based tool from the same HN thread [^1]
* [`fsevent_watch`](https://github.com/proger/fsevent_watch) -- via [alin23](https://notes.alinpanaitiu.com/Making%20macOS%20apps%20uninstallable) [^2]


---
[^1]: February 17, 2023 via HN https://news.ycombinator.com/item?id=34828012

[^2]: February 18, 2023 via HN https://news.ycombinator.com/item?id=34841742
