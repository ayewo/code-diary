# Miscellaneous Tips
## Progress Reporting in CLI Tools
TIL of [`progress`](https://github.com/Xfennec/progress) which can report progress for long running commands from the `coreutils` package (`cp`, `mv`, `dd`, `tar`, `gzip/gunzip`, `cat`, etc) on Linux via [HN - Linux tool to show progress for cp, mv, dd](https://news.ycombinator.com/item?id=36000407). The [discussion](https://news.ycombinator.com/item?id=36001812) also yielded another progress monitoring tool like `pv` (pipe viewer) and a other neat tool for network comms: `socat` (Socket cat).


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

## `ss`: Socket Statistcs
While creating a handful of `bash` shell `alias`es, I created an alias for `screen -ls` as `sls` and accidentally learned of the existence of `ss` which the `man` page calls: "ss - another utility to investigate sockets". It's described as "ss is used to dump socket statistics. It allows showing information similar to netstat.  It can display more TCP and state information than other tools."

<details>
<summary>Default output from <code>ss</code> (no args) looks informative (some entries were truncated)</summary>
<pre>
Netid          State           Recv-Q          Send-Q                                                                                          Local Address:Port                            Peer Address:Port              Process
u_str          ESTAB           0               0                                                                                                           * 19509                                      * 19075
u_str          ESTAB           0               0                                                                                                           * 50804616                                   * 50804617
u_dgr          ESTAB           0               0                                                                                                           * 16268                                      * 14442
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 892                                        * 14647
u_str          ESTAB           0               0                                                                                                           * 31573147                                   * 31573148
u_str          ESTAB           0               0                                                                                                           * 1042627                                    * 1042626
u_dgr          ESTAB           0               0                                                                                                           * 50804311                                   * 14442
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 27857                                      * 27856
u_str          ESTAB           0               0                                                                                                           * 29796                                      * 29801
u_str          ESTAB           0               0                                                                                                           * 17570                                      * 17569
u_str          ESTAB           0               0                                                                                                           * 31014                                      * 29920
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 16809                                      * 16235
u_str          ESTAB           0               0                                                                                 /run/dbus/system_bus_socket 19093                                      * 19550
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 17191                                      * 17188
u_str          ESTAB           0               0                                                                                                           * 50806057                                   * 50806056
u_str          ESTAB           0               0                                                                                                           * 14647                                      * 892
u_str          ESTAB           0               0                                                                                                           * 16033                                      * 16415
u_str          ESTAB           0               0                          /run/containerd/s/8b5a47b628f96058ef7e8cfeab22a42a782f01228abe8ad3526692d09eed5037 24429                                      * 24980
u_str          ESTAB           0               0                                                                                                           * 50806071                                   * 50806072
u_str          ESTAB           0               0                                                                                                           * 50806069                                   * 50806070
u_str          ESTAB           0               0                                                                                 /run/dbus/system_bus_socket 17115                                      * 17114
u_str          ESTAB           0               0                                                                                                           * 833                                        * 902
u_str          ESTAB           0               0                                                                                                           * 652474                                     * 652487
u_dgr          ESTAB           0               0                                                                                                           * 19520                                      * 14442
u_str          ESTAB           0               0                                                                                                           * 1041352                                    * 1041351
u_dgr          ESTAB           0               0                                                                                                           * 15941940                                   * 14442
u_str          ESTAB           0               0                          /run/containerd/s/fc65177fdeb90485556dfc68f6e248f085b418449bc63cce1f0d654b579d204c 31576174                                   * 31576172
u_str          ESTAB           0               0                                                                                                           * 16246                                      * 16877
u_str          ESTAB           0               0                                                                                                           * 17773                                      * 17774
u_str          ESTAB           0               0                                                                                                           * 1040016                                    * 1040017
u_str          ESTAB           0               0                                                                                                           * 27512                                      * 27520
u_str          ESTAB           0               0                                                                                                           * 24980                                      * 24429
u_str          ESTAB           0               0                                                                                                           * 16175                                      * 16272
u_str          ESTAB           0               0                                                                                 /run/dbus/system_bus_socket 16271                                      * 16636
u_str          ESTAB           0               0                                                                                                           * 50803706                                   * 0
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 15941939                                   * 15941182
u_str          ESTAB           0               0                                                                                                           * 21657                                      * 21658
u_str          ESTAB           0               0                                                                                                           * 50806063                                   * 50806062
u_str          ESTAB           0               0                                                                                                           * 17599                                      * 17600
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 30460                                      * 32771
u_str          ESTAB           0               0                                                                                                           * 50804618                                   * 50804619
u_dgr          ESTAB           0               0                                                                                                           * 19092                                      * 14444
u_str          ESTAB           0               0                                                                                                           * 17068                                      * 17071
u_str          ESTAB           0               0                                                                                                           * 17569                                      * 17570
u_str          ESTAB           0               0                                                                                                           * 31576105                                   * 31576108
u_str          ESTAB           0               0                                                                                                           * 1040017                                    * 1040016
u_dgr          ESTAB           0               0                                                                                                           * 16425                                      * 14444
u_dgr          ESTAB           0               0                                                                                         /run/systemd/notify 599                                        * 0
u_str          ESTAB           0               0                                                                                                           * 50805307                                   * 50805308
u_str          ESTAB           0               0                                                                                                           * 50804340                                   * 50804341
u_str          ESTAB           0               0                                                                                                           * 50806066                                   * 50806065
u_str          ESTAB           0               0                                                                                                           * 17770                                      * 17771
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 16661                                      * 16214
u_str          ESTAB           0               0                                                                                                           * 16270                                      * 16269
u_str          ESTAB           0               0                                                                                 /run/dbus/system_bus_socket 656567                                     * 657422
u_dgr          ESTAB           0               0                                                                                                           * 16971                                      * 14444
u_str          ESTAB           0               0                                                                                 /run/dbus/system_bus_socket 17162                                      * 17161
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 19075                                      * 19509
u_dgr          ESTAB           0               0                                                                                /run/systemd/journal/dev-log 14442                                      * 0
u_str          ESTAB           0               0                          /run/containerd/s/55c183969887742c489d1fb9f9dfcaf3cb957ba5058706e9615b53957b17cae5 31576173                                   * 31576171
u_str          ESTAB           0               0                                                                                          /run/user/1000/bus 22543                                      * 21504
u_dgr          ESTAB           0               0                                                                                 /run/systemd/journal/socket 14444                                      * 0
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 16894                                      * 16722
u_str          ESTAB           0               0                                                                                                           * 50104846                                   * 50104847
u_str          ESTAB           0               0                                                                                                           * 1042600                                    * 1042601
u_str          ESTAB           0               768                                                                                                         * 1039993                                    * 1039994
u_dgr          ESTAB           0               0                                                                                                           * 600                                        * 601
u_str          ESTAB           0               0                                                                                                           * 19550                                      * 19093
u_str          ESTAB           0               0                                                                                                           * 31576513                                   * 31575831
u_str          ESTAB           0               0                                                                                                           * 31575937                                   * 31575938
u_str          ESTAB           0               0                                                                                                           * 15941195                                   * 15941946
u_str          ESTAB           0               0                                                                                                           * 22540                                      * 22541
u_str          ESTAB           0               0                                                                                                           * 17600                                      * 17599
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 31664                                      * 30229
u_str          ESTAB           0               0                                                                                                           * 50806056                                   * 50806057
u_dgr          ESTAB           0               0                                                                                                           * 50112073                                   * 14442
u_str          ESTAB           0               0                                                                                                           * 1042601                                    * 1042600
u_dgr          ESTAB           0               0                                                                                                           * 14671                                      * 14444
u_str          ESTAB           0               0                                                                                                           * 50804620                                   * 50804621
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 16877                                      * 16246
u_str          ESTAB           0               0                                                                                                           * 31827                                      * 31832
u_str          ESTAB           0               0                                                                                                           * 16682                                      * 16804
u_str          ESTAB           0               0                                                                                                           * 30229                                      * 31664
u_str          ESTAB           0               0                                                                                 /run/dbus/system_bus_socket 15941946                                   * 15941195
u_str          ESTAB           0               0                                                                                                           * 17114                                      * 17115
u_str          ESTAB           0               0                                                                                                           * 31576171                                   * 31576173
u_str          ESTAB           0               0                                                                                                           * 27856                                      * 27857
u_str          ESTAB           0               0                                                                                                           * 1042602                                    * 1042603
u_dgr          ESTAB           0               0                                                                                                           * 16434                                      * 16433
u_str          ESTAB           0               0                                                                                                           * 50112106                                   * 50112107
u_str          ESTAB           0               0                                                                                                           * 50804619                                   * 50804618
u_str          ESTAB           0               0                                                                             /run/containerd/containerd.sock 17774                                      * 17773
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 31575831                                   * 31576513
u_str          ESTAB           0               0                                                                                                           * 16679                                      * 16780
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 31574175                                   * 31573387
u_str          ESTAB           0               0                                                                                                           * 50104847                                   * 50104846
u_str          ESTAB           0               0                                                                                                           * 50806067                                   * 50806068
u_str          ESTAB           0               0                                                                                                           * 31576172                                   * 31576174
u_str          ESTAB           0               0                                                                                                           * 50804621                                   * 50804620
u_str          ESTAB           0               0                                                                                                           * 1040018                                    * 1040019
u_str          ESTAB           0               0                          /run/containerd/s/0f542bdc9c22e9e4481cb5ab3206df1cc544199980fa5e4d8747be48a682b6b4 25503                                      * 25494
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 16464                                      * 16060
u_dgr          ESTAB           0               0                                                                                                           * 16432                                      * 16431
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 25061                                      * 24520
u_str          ESTAB           0               0                                                                                                           * 50806059                                   * 50806058
u_str          ESTAB           0               0                                                                                                           * 1039998                                    * 1039997
u_dgr          ESTAB           0               0                                                                                                           * 16433                                      * 16434
u_str          ESTAB           0               0                                                                                                           * 17148                                      * 17566
u_str          ESTAB           0               0                                                                                                           * 50806072                                   * 50806071
u_str          ESTAB           0               0                                                                                                           * 50806065                                   * 50806066
u_str          ESTAB           0               0                                                                                                           * 50804341                                   * 50804340
u_seq          ESTAB           0               0                                                                                                           * 16992                                      * 16991
u_dgr          ESTAB           0               0                                                                                                           * 16650                                      * 14442
u_str          ESTAB           0               0                          /run/containerd/s/ebc77d313d1cbc13c287475c88361956f2c0e58d874b0f5f6a235cb1a3155de3 31832                                      * 31827
u_str          ESTAB           0               0                                                                                                           * 16235                                      * 16809
u_str          ESTAB           0               0                                                                                                           * 657422                                     * 656567
u_str          ESTAB           0               0                                                                                                           * 1039996                                    * 1039995
u_str          ESTAB           0               0                                                                                                           * 50797614                                   * 50797615
u_dgr          ESTAB           0               0                                                                                                           * 16471                                      * 14444
u_str          ESTAB           0               0                                                                                                           * 21504                                      * 22543
u_str          ESTAB           0               0                                                                                                           * 16214                                      * 16661
u_str          ESTAB           0               0                          /run/containerd/s/b921780f42c26aba2f2518a293c04e57d873a41b5a15a8933b02e7a6928def89 29801                                      * 29796
u_str          ESTAB           0               0                                                                                                           * 16722                                      * 16894
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 652487                                     * 652474
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 16804                                      * 16682
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 31575938                                   * 31575937
u_dgr          ESTAB           0               0                                                                                                           * 19546                                      * 19547
u_str          ESTAB           0               0                                                                                                           * 16983                                      * 17458
u_str          ESTAB           0               0                                                                                                           * 17188                                      * 17191
u_str          ESTAB           0               0                                                                                                           * 1042626                                    * 1042627
u_dgr          ESTAB           0               0                                                                                                           * 16431                                      * 16432
u_str          ESTAB           1               0                                                                                                           * 1039994                                    * 1039993
u_str          ESTAB           0               0                                                                                                           * 50806058                                   * 50806059
u_dgr          ESTAB           0               0                                                                                                           * 16219                                      * 14442
u_dgr          ESTAB           0               0                                                                                    /run/chrony/chronyd.sock 16990                                      * 0
u_str          ESTAB           0               0                                                                                                           * 29733                                      * 30768
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 27648                                      * 27647
u_str          ESTAB           0               0                                                                                                           * 1041351                                    * 1041352
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 16780                                      * 16679
u_str          ESTAB           0               0                                                                                                           * 31573387                                   * 31574175
u_str          ESTAB           0               0                                                                                                           * 50104211                                   * 0
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 31575977                                   * 31575976
u_dgr          ESTAB           0               0                                                                                                           * 17441                                      * 14442
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 17071                                      * 17068
u_str          ESTAB           0               0                                                                                                           * 1039997                                    * 1039998
u_str          ESTAB           0               0                                                                                                           * 1042598                                    * 1042599
u_str          ESTAB           0               0                                                                                                           * 50805308                                   * 50805307
u_str          ESTAB           0               0                                                                                                           * 16636                                      * 16271
u_str          ESTAB           0               0                                                                                                           * 50806386                                   * 50806385
u_str          ESTAB           0               0                                                                                                           * 50806068                                   * 50806067
u_str          ESTAB           0               0                                                                                                           * 15941182                                   * 15941939
u_str          ESTAB           0               0                                                                                                           * 50806070                                   * 50806069
u_str          ESTAB           0               0                                                                                                           * 1041349                                    * 1041350
u_str          ESTAB           0               0                                                                                                           * 50111835                                   * 0
u_str          ESTAB           0               0                                                                             /run/containerd/containerd.sock 17771                                      * 17770
u_str          ESTAB           0               0                                                                                                           * 16269                                      * 16270
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 902                                        * 833
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 21658                                      * 21657
u_dgr          ESTAB           0               0                                                                                                           * 22539                                      * 14442
u_str          ESTAB           0               0                                                                                                           * 50806060                                   * 50806061
u_str          ESTAB           0               0                                                                                                           * 17161                                      * 17162
u_str          ESTAB           0               0                                                                                                           * 50797615                                   * 50797614
u_dgr          ESTAB           0               0                                                                                                           * 601                                        * 600
u_str          ESTAB           0               0                                                                                                           * 17410                                      * 17411
u_str          ESTAB           0               0                                                                                                           * 50804617                                   * 50804616
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 29920                                      * 31014
u_str          ESTAB           0               0                                                                                                           * 16659                                      * 16273
u_str          ESTAB           0               0                                                                                                           * 32771                                      * 30460
u_dgr          ESTAB           0               0                                                                                                           * 19547                                      * 19546
u_str          ESTAB           0               0                                                                                                           * 22541                                      * 22540
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 17566                                      * 17148
u_str          ESTAB           0               0                          /run/containerd/s/ad2826608780f1f9c545a05f205841b25faf044bcbe013434af12d2de0bd76b6 28595                                      * 28590
u_dgr          ESTAB           0               0                                                                                                           * 688                                        * 14444
u_str          ESTAB           0               0                          /run/containerd/s/101a1e3acd6791f9e8c90c73a22fb430f4e03d7868efe7fade1cc9b8685e42a8 27520                                      * 27512
u_str          ESTAB           0               0                          /run/containerd/s/816f1f91d546fa8145dee95311445b7a6056192ccd57787822474c26fad23b4a 31576108                                   * 31576105
u_str          ESTAB           0               0                                                                                                           * 1039995                                    * 1039996
u_str          ESTAB           0               0                                                                                                           * 16060                                      * 16464
u_str          ESTAB           0               0                                                                                                           * 1042599                                    * 1042598
u_dgr          ESTAB           0               0                                                                                                           * 14676                                      * 14675
u_str          ESTAB           0               0                                                                                 /run/dbus/system_bus_socket 17458                                      * 16983
u_str          ESTAB           0               0                                                                                                           * 50804615                                   * 50804614
u_dgr          ESTAB           0               0                                                                                                           * 16262                                      * 14442
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 16759                                      * 16662
u_dgr          ESTAB           0               0                                                                                                           * 50104552                                   * 14442
u_str          ESTAB           0               0                                                                                                           * 50806062                                   * 50806063
u_str          ESTAB           0               0                          /run/containerd/s/12b03908d101a9f6538c60d9e9a5d20a1b6942854e5ce7a6d62a1cc67dba69c9 30033                                      * 31038
u_str          ESTAB           0               0                                                                                                           * 28590                                      * 28595
u_str          ESTAB           0               0                                                                                                           * 1042603                                    * 1042602
u_str          ESTAB           0               0                                                                                 /run/systemd/journal/stdout 16415                                      * 16033
u_dgr          ESTAB           0               0                                                                                                           * 14675                                      * 14676
u_str          ESTAB           0               0                          /run/containerd/s/60fefb031f75e1fc20952608f45d3893e1bcde166a17aeb73dab9a1c51f4bb82 31573148                                   * 31573147
u_str          ESTAB           0               0                                                                                                           * 25494                                      * 25503
u_str          ESTAB           0               0                                                                                                           * 24520                                      * 25061
u_str          ESTAB           0               0                                                                                                           * 31575976                                   * 31575977
u_str          ESTAB           0               0                                                                                                           * 50804614                                   * 50804615
u_str          ESTAB           0               0                                                                                                           * 31038                                      * 30033
u_str          ESTAB           0               0                                                                                                           * 16662                                      * 16759
u_str          ESTAB           0               0                                                                                                           * 50806385                                   * 50806386
u_str          ESTAB           0               0                                                                       /run/containerd/containerd.sock.ttrpc 30768                                      * 29733
u_str          ESTAB           0               0                                                                                                           * 27647                                      * 27648
u_str          ESTAB           0               0                                                                                                           * 50112107                                   * 50112106
u_str          ESTAB           0               0                                                                                                           * 1040019                                    * 1040018
u_str          ESTAB           0               0                                                                                                           * 50806061                                   * 50806060
u_str          ESTAB           0               0                                                                                 /run/dbus/system_bus_socket 16272                                      * 16175
u_dgr          ESTAB           0               0                                                                                                           * 14469                                      * 599
u_str          ESTAB           0               0                                                                                 /run/dbus/system_bus_socket 17411                                      * 17410
u_seq          ESTAB           0               0                                                                                                           * 16991                                      * 16992
u_str          ESTAB           0               0                                                                                                           * 1041350                                    * 1041349
u_str          ESTAB           0               0                                                                                 /run/dbus/system_bus_socket 16273                                      * 16659
icmp6          UNCONN          0               0                                                                                                      *%ens5:ipv6-icmp                                  *:*
tcp            ESTAB           0               0                                                                                                   127.0.0.1:33022                              127.0.0.1:35553
tcp            ESTAB           0               0                                                                                                   127.0.0.1:40568                              127.0.0.1:5173
</pre>
</details>

## Find Files with UTF-8 or ASCII Encoding
TIL of some neat CLI options to the `find` & `file` commands and learnt of `iconv` via [HN](https://news.ycombinator.com/item?id=39217149): [My favourite Git commit](https://dhwthompson.com/2019/my-favourite-git-commit):
```bash
# Where $folder is a Rails application folder
cd $folder/

find . -type f -exec file --mime {} \+ | grep utf
./app/mailers/application_mailer.rb:                                                      text/x-ruby; charset=utf-8
./app/views/root_mailer/mailer.html.erb:                                                  text/html; charset=utf-8
./app/views/root_mailer/mailer.text.erb:                                                  text/plain; charset=utf-8
./RESPONSES.md:                                                                           text/plain; charset=utf-8
...

find . -type f -exec file --mime {} \+ | grep us-ascii
...
./bin/rails:                                                                              text/x-ruby; charset=us-ascii
./config/routes.rb:                                                                       text/plain; charset=us-ascii
./config/locales/en.yml:                                                                  text/plain; charset=us-ascii
./config/cable.yml:                                                                       text/plain; charset=us-ascii
./config/environments/production.rb:                                                      text/plain; charset=us-ascii
./config/environments/development.rb:                                                     text/plain; charset=us-ascii
...

# Let's pick "./RESPONSES.md" for conversion from utf-8 to us-ascii using iconv:
file --mime ./RESPONSES.md
./RESPONSES.md: text/plain; charset=utf-8

iconv -f UTF8 -t US-ASCII ./RESPONSES.md 2>&1 | tail -n5
...

file --mime ./RESPONSES.md                              
./RESPONSES.md: text/plain; charset=us-ascii
```

## CLI Benchmarking Tool
TIL of [`hyperfine`](https://github.com/sharkdp/hyperfine) on Feb 3, 2024. hyperfine is a benchmarking tool written in Rust for comparing multiple runs of the same tool or different tools. Saw it used [here](https://github.com/gunnarmorling/1brc/discussions/569).

For consistent benchmarks on Linux their README mentions to clear the hard disk cache before each timing run:
```bash
sync; echo 3 | sudo tee /proc/sys/vm/drop_caches
```
Just days earlier (January 30, 2024), I encountered that exact command in this excellent article on the [Linux page cache](https://biriukov.dev/docs/page-cache/1-prepare-environment-for-experiments/). 

## See all versions of `time` installed on your system ...
This SO [comment](https://stackoverflow.com/questions/385408/get-program-execution-time-in-the-shell#comment86233029_385418) pointed me to a useful TIL:
`type -a time`:
```bash
# on Linux
type -a time
time is a shell keyword
time is /usr/bin/time
time is /bin/time

# on macOS (zsh)
type -a time
time is a reserved word
time is /usr/bin/time
```

## Set the birth time on file creation
I learnt of the `-t` flag to the `touch` command from this HN [comment](https://news.ycombinator.com/item?id=40490379):
```bash
touch -t 201101020304 /tmp/old-file

ll /tmp/old-file 
-rw-r--r--  1 mac  wheel  0 Jan  2  2011 /tmp/old-file
```
Tested on the macOS filesystem (APFS). You can check your mac's filesystem with `diskutil list`.

---

[^1]: <empty>

[^2]: <empty>
