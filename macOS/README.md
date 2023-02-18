# macOS Tips

## Disable Gatekeeper
I often think of the ability to quickly turn on/off Gatekeeper on macOS, so this [HN comment](https://news.ycombinator.com/item?id=34845211) using `spctl` (System Policy Control?) offers a handy tip[^1]:
```bash
	sudo spctl --master-disable
```
I'm still on macOS v12.6 Monterey but the help output from `spctl` only mentions the `--global-disable` flag. 
It seems `--master-disable` is an alias for `--global-disable`. 
When I do have a need for it, I will check to see if `sudo spctl --global-disable` produces the same outcome.


---
[^1]: February 18, 2023
