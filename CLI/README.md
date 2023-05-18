# Miscellaneous Tips
## Python Version Version?
I came across `python --version --version` in some script (can't remember where now) so I decided to check what it did:
```bash
python --version --version
Python 3.9.13 (main, Sep 30 2022, 08:09:46)
[Clang 14.0.0 (clang-1400.0.29.102)]
```
Compare that to what I'm used to seeing:
```bash
python --version
Python 3.9.13
```
## Simulate Memory Pressure on Linux
To trigger the OOM killer on Linux execute `tail /dev/zero`. Hat tip to Danny Lin on [Twitter](https://twitter.com/ayewo_/status/1645713535643578368).
