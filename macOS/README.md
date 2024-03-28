# macOS: Random Tips
## `networkQuality` tool
This HN thread [The networkQuality tool on macOS ](https://news.ycombinator.com/item?id=35936999) led to some interesting tips and tools.
* `networkQuality` [Swift and Go implementation](https://news.ycombinator.com/item?id=35937295);
* Useful list of tools inside [`/usr/sbin`](https://news.ycombinator.com/item?id=35937359);
* Miscellaneous [CLI and GUI tools](https://news.ycombinator.com/item?id=35937763). 


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

## Password Database
macOS offers three[^3] different UIs for viewing and managing the passwords in your password database.
* Keychain Access (CLI command to open it directly on macOS 12.6[^4])
* System Settings -> Passwords ([CLI command](https://news.ycombinator.com/item?id=35331649) to open it directly on macOS 12.6[^5])
* Safari Browser -> Preferences -> Passwords (example [AppleScript](https://news.ycombinator.com/item?id=35332262) to open it directly--not tested)

## [System Integrity Protection (SIP)](https://developer.apple.com/documentation/security/disabling_and_enabling_system_integrity_protection)
Overheard on the Internet[^6]:
> iOS apps don't work with SIP disabled.

## How many CPU Cores are on your Mac?
While reading [1 billion row challenge in SQL and Oracle Database](https://geraldonit.com/2024/01/31/1-billion-row-challenge-in-sql-and-oracle-database/) I was curious how many cores I had on my Intel MBP. 

I used `sysctl -n hw.ncpu`[^7].
```bash
mac@13-inch-MacBook-Pro ~ % sysctl -n hw.ncpu
8
```
So my MBP has 8 cores. **But**, the `sysctl -a` has more info:
```bash
hw.ncpu: 8
...
hw.activecpu: 8
...
hw.physicalcpu: 4
hw.physicalcpu_max: 4
hw.logicalcpu: 8
hw.logicalcpu_max: 8
...
hw.targettype: Mac
hw.cputhreadtype: 1
...
```

To identify what generation of Intel® Core™ processor came with your Mac, use `sysctl -a | grep machdep.cpu.brand_string`[^8].
```bash
mac@13-inch-MacBook-Pro ~ % sysctl -a | grep machdep.cpu.brand_string
machdep.cpu.brand_string: Intel(R) Core(TM) i5-1038NG7 CPU @ 2.00GHz
```
In my case, the [`Intel(R) Core(TM) i5-1038NG7`](https://www.intel.com/content/www/us/en/products/sku/196594/intel-core-i51038ng7-processor-6m-cache-up-to-3-80-ghz/specifications.html) in the output implies that the processor is [10th generation](https://www.intel.com/content/www/us/en/support/articles/000032203/processors/intel-core-processors.html) because the number 10 is listed after i5.

## How to get Eclipse Corrosion working on your Mac?
I wanted to try out Eclipse Corrosion which is an IDE that can debug Rust programs using `rust-gdb`. It's based on the slowly dying Eclipse IDE platform from about 20 years ago.

I kept getting an error on double-click that the app was damaged so I manually extracted the `Eclipse.app` from the download bundle then cross-checked the file hash, before remembering that perhaps the app might have been quarantined by Gatekeeper.

* Eclipse Corrosion [download page](https://download.eclipse.org/corrosion/releases/latest/products/); 
* This [interstitial page](https://www.eclipse.org/downloads/download.php?file=/corrosion/releases/latest/products/eclipseide-rust-1.2.4-macosx.cocoa.x86_64.tar.gz) shows the file hash (in `SHA-512`).
```bash
# file hash in SHA-512:
189f36c81bb4cc53cee160240006d2bd62400b2e414dab4a82d69424da1dc8644fba62aa546e08ac723562f50c5952bfc221ea9aff13ede26b7477117e579a72  eclipseide-rust-1.2.4-macosx.cocoa.x86_64.tar.gz

# confirm file hash using `shasum`:
shasum -a 512 ~/Downloads/eclipseide-rust-1.2.4-macosx.cocoa.x86_64.tar.gz 
189f36c81bb4cc53cee160240006d2bd62400b2e414dab4a82d69424da1dc8644fba62aa546e08ac723562f50c5952bfc221ea9aff13ede26b7477117e579a72  ~/Downloads/eclipseide-rust-1.2.4-macosx.cocoa.x86_64.tar.gz

cd ~/Downloads/
tar xf eclipseide-rust-1.2.4-macosx.cocoa.x86_64.tar.gz 
open Eclipse.app

# fails to open. Complains about the app being damaged. Had to remove what is likely a quarantine flag by macOS
xattr -d com.apple.quarantine Eclipse.app 

# works!
open Eclipse.app
```

---
[^1]: February 17, 2023 via [HN](https://news.ycombinator.com/item?id=34828012) -> [Sloth](https://github.com/sveinbjornt/Sloth/issues/22) -> [Balena](https://github.com/jorangreef/sudo-prompt/issues/53) -> [sudo-prompt](https://github.com/jorangreef/sudo-prompt/blob/c3cc31a51bc50fe21fadcbf76a88609c0c77026f/README.md#invalidating-the-timestamp). Along the [way](https://github.com/balena-io/etcher/issues/2644#issuecomment-619969067) I learned that Linux has a subsystem for managing device events called `udev` that allows automation. Scripts can be triggered when a specific device is plugged in. This was a good primer: [An introduction to Udev: The Linux subsystem for managing device events](https://opensource.com/article/18/11/udev).

[^2]: February 18, 2023 via [HN](https://news.ycombinator.com/item?id=34841742) 

[^3]: [*Apple passwords deserve an app*](https://news.ycombinator.com/item?id=35329950), March 28, 2023 via [HN](https://news.ycombinator.com/item?id=35330427)

[^4]: Apple uses indirection for apps paths under `Applications/Utilities`; dragging from Finder into Terminal revealed the real path is prefixed with `/System/...`: `open /System/Applications/Utilities/Keychain\ Access.app` 

[^5]: -ditto- via [HN](https://news.ycombinator.com/item?id=35331649): `open /Library/Apple/System/Library/CoreServices/SafariSupport.bundle/Contents/PreferencePanes/Passwords.prefPane`

[^6]: [The day Windows died](https://news.ycombinator.com/item?id=35415758), April 3, 2023 via [from](https://news.ycombinator.com/item?id=35416683)

[^7]: February 3, 2024 via [How to check the status of hyper-threading in macOS](https://discussions.apple.com/thread/250367139?sortBy=best) -> [macincloud.com](https://support.macincloud.com/support/solutions/articles/8000087401-how-can-i-check-the-number-of-cpu-cores-on-a-mac)

[^8]: -ditto- via https://www.intel.com/content/www/us/en/support/articles/000006059/processors.html 
