# macOS: Random Tips
## Invalidate `sudo` timestamp
Force user re-authentication for `sudo` with [^1]:
```bash
sudo -k
```
## Disable Gatekeeper
I often think of the ability to quickly turn on/off Gatekeeper on macOS, so this [HN comment](https://news.ycombinator.com/item?id=34845211) using `spctl` (System Policy Control?) offers a handy tip[^2]:
```bash
	sudo spctl --master-disable
```
I'm still on macOS v12.6 Monterey but the help output from `spctl` only mentions the `--global-disable` flag. 
It seems `--master-disable` is an alias for `--global-disable`. 
When I do have a need for it, I will check to see if `sudo spctl --global-disable` produces the same outcome.

## [Using dtrace on MacOS with SIP enabled](https://poweruser.blog/using-dtrace-with-sip-enabled-3826a352e64b)
To better understand the relationship between [dtrace and SIP](https://www.google.com/search?client=safari&rls=en&q=dtrace+macos&ie=UTF-8&oe=UTF-8)[^2], I landed on the above article.
The article mentioned a GUI tool [Crescendo](https://github.com/SuprHackerSteve/Crescendo) similar to Microsoft Sysinternal’s Procmon. And from the [repo](https://github.com/SuprHackerSteve/Crescendo/tree/836ca3402b24564a3ecc6096883e480de7ad62e8#testing-and-development) I learnt of the `systemextensionsctl` binary on macOS. I tried the `systemextensionsctl list` command:
```bash
systemextensionsctl list
2 extension(s)
--- com.apple.system_extension.network_extension
enabled active  teamID  bundleID (version)  name  [state]
    VBG97UB4TA  com.objective-see.lulu.extension (2.4.0/2.4.0)  Extension [terminated waiting to uninstall on reboot]
* * VBG97UB4TA  com.objective-see.lulu.extension (2.4.2/2.4.2)  Extension [activated enabled]
```
which listed the same app (LuLu) twice because I haven't rebooted since I updated to a new version.
```bash
uptime
15:52  up 28 days, 37 mins, 11 users, load averages: 1.85 1.97 1.9
```
The [repo](https://github.com/SuprHackerSteve/Crescendo/tree/836ca3402b24564a3ecc6096883e480de7ad62e8#troubleshooting) also says towards the end that: "many macOS applications are launched via `xpcproxy`".

---
[^1]: February 17, 2023 via [HN](https://news.ycombinator.com/item?id=34828012) -> [Sloth](https://github.com/sveinbjornt/Sloth/issues/22) -> [Balena](https://github.com/jorangreef/sudo-prompt/issues/53) -> [sudo-prompt](https://github.com/jorangreef/sudo-prompt/blob/c3cc31a51bc50fe21fadcbf76a88609c0c77026f/README.md#invalidating-the-timestamp). Along the [way](https://github.com/balena-io/etcher/issues/2644#issuecomment-619969067) I learned that Linux has a subsystem for managing device events called `udev` that allows automation. Scripts can be triggered when a specific device is plugged in. This was a good primer: [An introduction to Udev: The Linux subsystem for managing device events](https://opensource.com/article/18/11/udev).

[^2]: February 18, 2023 via [HN](https://news.ycombinator.com/item?id=34841742) 
