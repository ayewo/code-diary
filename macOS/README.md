# macOS: Random Tips

## Disable Gatekeeper
I often think of the ability to quickly turn on/off Gatekeeper on macOS, so this [HN comment](https://news.ycombinator.com/item?id=34845211) using `spctl` (System Policy Control?) offers a handy tip[^2]:
```bash
	sudo spctl --master-disable
```
I'm still on macOS v12.6 Monterey but the help output from `spctl` only mentions the `--global-disable` flag. 
It seems `--master-disable` is an alias for `--global-disable`. 
When I do have a need for it, I will check to see if `sudo spctl --global-disable` produces the same outcome.

## Invalidate `sudo` timestamp
Force user re-authentication for `sudo` with [^1]:
```bash
sudo -k
```


---
[^1]: February 17, 2023 via [Sloth](https://github.com/sveinbjornt/Sloth/issues/22) -> [Balena](https://github.com/jorangreef/sudo-prompt/issues/53) -> [sudo-prompt](https://github.com/jorangreef/sudo-prompt/blob/c3cc31a51bc50fe21fadcbf76a88609c0c77026f/README.md#invalidating-the-timestamp)

[^2]: February 18, 2023
