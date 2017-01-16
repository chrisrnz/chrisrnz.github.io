---
title: "Useful bash one-liners"
published: true
---

# Some useful bash one liners

## print interfaces

```bash
ip route list scope link | awk '{print $3}'
```

## print connected networks

```bash
ip route list scope link | awk '{print $1}'
```

## print both

```bash
ip route list scope link | awk '{print $3,$1}'
```

## use it in a loop and figure out what network driver we're using

```bash
for INTERFACE in $(ip route list scope link | awk '{print $3}'); do echo ${INTERFACE} ;ethtool --driver ${INTERFACE} | grep driver | awk '{print $2}' ;done
```

## delete files in /dir/ older than a year

```bash
find /dir/ -mtime +365 -exec rm -v {} \;
```
