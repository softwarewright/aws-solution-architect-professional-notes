# AWS Storage Gateway

https://www.youtube.com/watch?v=9wgaV70FeaM

Allowed protocols

- NFS v3/v4
- iSCSI

Options

Tape
- Additional backup apps
- Improved performance enhancements
- 3 to 5 hour retrieval from Amazon Glacier
- Supports leading backup applications
- 1 PB storage per gateway with unlimited archive

Volume
- Cloning for faster DR

File Gateway
- Refresh cache for multi-site file sharing
- Upload notifications

## Volumed Gateway

All data that leaves the Storage Gateway appliance is encrypted until it reaches its destination.

### Volume Stored

100% of the data is on the VM, this is for situations where you cannot afford the cache hit. Where you need all of the information readily available.

Reads and writes happen directly on the disk.

### Volume Cached

There a portion of your data that is cached on the VM and then the rest is uploaded directly to AWS S3

### File Gateway

Creates a 1:1 mapping files to objects. You can get upload notifications on your data.

Reads will happen through the cache and they will fetch the file if it is missing as intelligently as possible. 



## Usage of Volume Gateway Cached

### Monitoring

- Per-volume monitoring
    - cachehitpercentage (80%) any higher and you need to scale up
    - cachepercentdirty (2.5%)
    - cachepercentused
- Monitor upload buffer
- Gateway read/write times
    - Shows performance issues with hardware
    - Correlate with VMware
- Tune alerts based on workload
- Review monthly cache averages for sizing/growth

### Backup and Restore

EBS snapshots are created for backup through the storage gateway. You can use lambdas to clean up old snapshots.

Using a warm standby gateway and file server in AWS. This can achieve 10min data availability and 30min RTO.

### Gateway Sizing

Hardware 
- Make it fast and dedicated
- Lots of CPU, to avoid contention
- 16GB memory is fine
- SSD or SAS with good controllers
- Building NVMe-based GW

Volumes and Disk
- Split disk pools for upload buffer, cache, and OS
- Size upload buffer by your internet connection
- Size cache by data ingest, use, and turn rate

## Resources 
- [CloudEndure](https://aws.amazon.com/disaster-recovery/)

## Challenge

- Use storage gateway with an EC2