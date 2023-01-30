# January 29, 2023
[Ask HN: What is the best source to learn Docker in 2023?](https://news.ycombinator.com/item?id=34563353) threw up some interesting links:
* https://www.schutzwerk.com/en/blog/linux-container-intro/ 
* https://jvns.ca/blog/2020/04/27/new-zine-how-containers-work/


# January 9, 2023
Came across this article: [Docker on MacOS is slow and how to fix it](https://www.paolomainardi.com/posts/docker-performance-macos/) via Changelog weekly newsletter on January 8, 2023 and decide to use `lima` instead of `docker`.

```bash
lima nerdctl run -i ubuntu
exit

lima nerdctl run -it ubuntu
root@bf4ad6e01ee8:/# exit
exit

lima nerdctl run -d ubuntu
59c6265a14cba05b481ffe7ad42286ef016068a489d5dbde7f51842abce2787

lima nerdctl inspect --format '{{ .State.Pid }}' 59c6265a14cba05b481ffe7ad42286ef016068a489d5dbde7f51842abce2787f
WARN[0000] failed to inspect NetNS                       error="failed to Statfs \"/proc/18750/ns/net\": no such file or directory" id=59c6265a14cba05b481ffe7ad42286ef016068a489d5dbde7f51842abce2787f
18750

## From https://www.paolomainardi.com/posts/docker-performance-macos/ but only works on docker not lima
# Find the host path of the container filesystem (in this case is a btrfs volume)
sudo cat /proc/18750/mountinfo | grep subvolumes
```

**Free up space used by `lima`'s cache**
```bash
cd /Users/mac/Library/Caches/lima/download/by-url-sha256

du -shL fdbae42d5655d51afaedd0563a278bc93ea15d4f90aa8722164682cb77fff619 \
3304d173f1e1824e5be6cf84bf2f78825cf0db416c4c975dbb2458776942629e \
62c871f6f494814b9c6555e6ac74e1e81c35ba6f12beec1a0d74c7bee5c8a41d \
e1fed960ebd29619676c7ab7535bc83f7fb2ad71739edb6fde4e17bce0b61a47 \
dd4613292b3a598687b9e5f97a294845661491c61e554435a27c6f6c691f8dd4

175M  fdbae42d5655d51afaedd0563a278bc93ea15d4f90aa8722164682cb77fff619
173M  3304d173f1e1824e5be6cf84bf2f78825cf0db416c4c975dbb2458776942629e
4.0K  62c871f6f494814b9c6555e6ac74e1e81c35ba6f12beec1a0d74c7bee5c8a41d
553M  e1fed960ebd29619676c7ab7535bc83f7fb2ad71739edb6fde4e17bce0b61a47
218M  dd4613292b3a598687b9e5f97a294845661491c61e554435a27c6f6c691f8dd4

rm -rf fdbae42d5655d51afaedd0563a278bc93ea15d4f90aa8722164682cb77fff619 
rm -rf 3304d173f1e1824e5be6cf84bf2f78825cf0db416c4c975dbb2458776942629e 
rm -rf 62c871f6f494814b9c6555e6ac74e1e81c35ba6f12beec1a0d74c7bee5c8a41d 
rm -rf e1fed960ebd29619676c7ab7535bc83f7fb2ad71739edb6fde4e17bce0b61a47 
rm -rf dd4613292b3a598687b9e5f97a294845661491c61e554435a27c6f6c691f8dd4
```

