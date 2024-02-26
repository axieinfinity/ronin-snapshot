# Ronin Snapshot

## Note
- We capture a comprehensive snapshot of **full node data**, including pruned state data, once per week.

## Prerequisites
- Your free disk space has more than twice the size of the snapshot.
- Install the [zstd](https://github.com/facebook/zstd) on your machine.
- Install the [tmux](https://github.com/tmux/tmux/wiki/Installing) for long time process operation.


### Endpoint

- mainnet: [chaindata-20240222.tar.zst](https://pub-3cca138de6c349f8afe5f6635f9f6f81.r2.dev/data/chaindata-20240222.tar.zst) - md5: 259c64b6393a6e5bceb9930062eca7cf
- testnet: [testnet-chaindata-20240220.tar.zst](https://pub-3cca138de6c349f8afe5f6635f9f6f81.r2.dev/data/testnet-chaindata-20240220.tar.zst) - md5: 390dfa12f343800e862d751661278a77

### Usage

Step 1: Preparation
- Make sure your hardware meets the [suggested requirement](https://docs.roninchain.com/docs/node-operators/mainnet/non-validator#install-the-node).
- A disk with enough free storage, at least twice the size of the snapshot.

Step 2: Download & Uncompress
- Copy the above snapshot URL.
- Download:  `wget -O chaindata.tar.zst  "<paste snapshot URL here>"` . It will take one or two hours to download the snapshot, you can put it in to the `tmux` by `wget -O chaindata.tar.gz  "<paste snapshot URL here>"`


* [OPTIONAL] If you need to speedup download, just use [aria2c](https://github.com/aria2/aria2)
```
aria2c -o chaindata.tar.zst -s14 -x14 -k100M https://pub-3cca138de6c349f8afe5f6635f9f6f81.r2.dev/data/{filename}
```

But aria2c may fail sometimes, you need to rerun the download command. To make it convient, you can use the following script, save it into file `download.sh`, open new `tmux` session and run: `chmod +x download.sh && ./download.sh "<paste snapshot URL here>" <your dir>`
```
#!/bin/bash
if [ $# -eq 1 ]; then
        dir=$(pwd)
elif [ $# -eq 2 ]; then
        dir=$2
else
        echo "Usage: $0 <uri> [filepath] "
        exit 1
fi
uri=$1
filename=$(basename "$uri")
status=-1
while (( status != 0 ))
do
        PIDS=$(pgrep aria2c)
        if [ -z "$PIDS" ]; then
                aria2c -d $dir -o $filename -s14 -x14 -k100M $uri
        fi
        status=$?
        pid=$(pidof aria2c)
        wait $pid
        echo aria2c exit.
        case $status in
                3)
                        echo file not exist.
                        exit 3
                        ;;
                9)
                        echo No space left on device.
                        exit 9
                        ;;
                *)
                        continue
                        ;;
        esac
done
echo download succeed.
exit 0
```

- Performance pretty good compare to `wget` command:

```
[#daede1 145GiB/145GiB(99%) CN:1 DL:115MiB]
10/05 10:34:40 [NOTICE] Download complete: /axie/geth.tar.zst

Download Results:
gid   |stat|avg speed  |path/URI
======+====+===========+=======================================================
daede1|OK  |   207MiB/s|/axie/geth.tar.zst

Status Legend:
(OK):download completed.

real    12m2.862s
user    1m57.320s
sys     2m28.624s
```

- Uncompress: `tar -I zstd -xvf chaindata.tar.zst`. It will take more than 20 min to uncompress. You can put it in the `tmux` session and run command `tar -I zst -xvf chaindata.tar.zst`
- You can combine the above steps by running a script:

```
wget -O chaindata.tar.zst  "<paste snapshot URL here>"
tar -I zstd -xvf chaindata.tar.zst
```


- If you do not need to store the archive for use with other nodes, you may also extract it while downloading to save time and disk space:
```
wget -q -O - <snapshot URL> | tar -I zstd -xvf -
```


Step 3: Install the node
- Now you can follow steps by steps from here [Install the node ](https://docs.roninchain.com/docs/node-operators/mainnet/non-validator#install-the-node)
- This docs is the detail for `6.(Optional) Download the snapshot`

### Endpoint(Mainnet): update every 3 months
- archive-mainnet-000: [archive-mainnet-chaindata-20240220.tar.zst-000](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-000) - md5: d790403804d40371047dcf228ac8fd01
- archive-mainnet-001: [archive-mainnet-chaindata-20240220.tar.zst-001](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-001) - md5: 71f1bb061e5fc2436e57ce783c164c5f
- archive-mainnet-002: [archive-mainnet-chaindata-20240220.tar.zst-002](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-002) - md5: 25e78ab8b30762d54331ca1b90536282
- archive-mainnet-003: [archive-mainnet-chaindata-20240220.tar.zst-003](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-003) - md5: be895d287fcb945d42c113e1eccac7df
- archive-mainnet-004: [archive-mainnet-chaindata-20240220.tar.zst-004](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-004) - md5: 65aea03ff022e784adeb81cc3028c87a
- archive-mainnet-005: [archive-mainnet-chaindata-20240220.tar.zst-005](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-005) - md5: 883f53e66cf1775d29ac114e03a0d67a
- archive-mainnet-006: [archive-mainnet-chaindata-20240220.tar.zst-006](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-006) - md5: 7cf7f925c066683728ae966fdad9f723
- archive-mainnet-007: [archive-mainnet-chaindata-20240220.tar.zst-007](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-007) - md5: 3645bf6bc8102254237eab0a3eb2c503
- archive-mainnet-008: [archive-mainnet-chaindata-20240220.tar.zst-008](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-008) - md5: 3fdf7392d092d67a3df64e48018abb14
- archive-mainnet-009: [archive-mainnet-chaindata-20240220.tar.zst-009](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-009) - md5: bf59656e380acde4b0d64e623bde6f2b
- archive-mainnet-010: [archive-mainnet-chaindata-20240220.tar.zst-010](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-010) - md5: e71731679c5ef96076f330d925f6adff
- archive-mainnet-011: [archive-mainnet-chaindata-20240220.tar.zst-011](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-011) - md5: f2b46bd18644b958bd2137b103198c7f
- archive-mainnet-012: [archive-mainnet-chaindata-20240220.tar.zst-012](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-012) - md5: b0a207fda18ce09c6aa41bb1829deb53
- archive-mainnet-013: [archive-mainnet-chaindata-20240220.tar.zst-013](https://storage.googleapis.com/sm-ronin-snapshot/archive-mainnet-chaindata-20240220.tar.zst-013) - md5: d0c9ca7bbe5f8f3b4f777f8f423fda2c


### Usage
Download && Concatenate && Uncompress
```shell
wget -O archive-mainnet-chaindata-20240220.tar.zst-xxx  "<paste snapshot URL here>" 
cat "archive-mainnet-chaindata-20240125.tar.zst-"* > combined_compressed_file.tar.zst
tar -I zstd -xvf chaindata.tar.zst
```
