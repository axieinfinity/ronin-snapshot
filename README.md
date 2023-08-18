# ronin-snapshot
Compressed database, block number = `0x1998c5f`.

## Prerequisites
- Your free disk space has more than twice the size of the snapshot.

## Uncompress snapshot
1. Download chaindata and checksum:
```shell
curl -O -L -k https://storage.googleapis.com/chaindata/chaindata-0x1998c5f.tar
curl -O -L -k https://storage.googleapis.com/chaindata/checksum-0x1998c5f.md5
md5sum -c checksum-0x1998c5f.md5
```
2. Uncompress downloaded files:
```shell
tar -xvf chaindata-0x1998c5f.tar
 ```
3. Stop bridge and node:
```shell
docker stop bridge
docker stop -t 300 node
```

4. Replace data:
- Consider backing up chaindata if you have run Ronin node before:
```shell
mv ~/.skymavis/chaindata/data/ronin/chaindata ~/.skymavis/chaindata/data/ronin/chaindata_backup
mv ~/.skymavis/chaindata/data/ronin/triecache ~/.skymavis/chaindata/data/ronin/triecache_backup
# or
rm -rf ~/.skymavis/chaindata
```
- Move the new uncompressed directory into the right place:
```shell
mkdir -p ~/.skymavis/chaindata/data/ronin
mv chaindata ~/.skymavis/chaindata/data/ronin
```

5. Restart Ronin services:
```shell
docker start node
docker start bridge
```

6. After some minutes, verify your node is connecting and up to date with the network at [stats.roninchain.com](https://stats.roninchain.com/).

7. Optionally remove the downloaded files.
