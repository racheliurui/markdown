title: AWS - EBS
date: 2018-06-13 19:02:00
tags:
- AWS
- AWS EBS
---

# EBS definition

* Network Block Storage
* 5 9s availability
* Attached to EC2 in same AZ
* point-in-time backup to S3

# Terminology

* iops : transaction /sec
* Throughput: read/write / sec
* Latency: delay between request response
* Capacity: Volumn size
* Block size: size of each i/o (kb)

![GP2 Bursting Diagram][https://github.com/racheliurui/markdown/blob/master/AWS/AWS2018/images/extra_EBS_gp2.png?raw=true]

* Scenario: boost time speed up for windows when using gp2 because of Bursting
  * which is important autosclaing
* Database ( you can use 2 gp2 volumn to archive 2*3k iops)

# History

With EC2 -> Magnet Storage -> SSD Storage -> gp2 SSD Storage -> Volumn Encryption -> Larger, faster -> Boot volumn encryption -> st1 and sc1 (HDD) -> __EBS Elastic Volumns__ (2017)

## IOPS vs Throughput
Select the correct type
IOPS: gp2 io1
Throughput: st1 sc1

## EBS Elastic Volumns

Change:
* You can increase volumn size (can't decrease)
* You can change volumn type
* You can increase or decrease IOPS
* You can combine above change together

Limitation:
* change type must be valid , like size , st1 min size =500G, so we can't change to st1 type with a size <500G

|  | Type | MinSize| MaxSize| Min IOPS| Max IOPS| Min Throughput| Max Throughput |
|----|----|----| ----|----|----|----|----|
|io1 | SSD | 1GiB |16TiB| 100 | 3K|
|gp2 | SSD | 4GiB| 16TiB| 100| 20K (50:1)|
|sc1  | Magnet| 500GiB| 16TiB| N/A  | N/A |12|192|
|st1   | Magnet| 500GiB| 16TiB |N/A|N/A| 40|500|


### How to modify elastic Volumns

* Cli

```aws cli

aws ec2 modify-volumn
[--dry-run | --no-dry-run]
--volumn-id <value>
[--size <value>]
[--volumn-type <value>]
[--iops <value>]

```

* other options: SDK, web console

### Benefits

* No performance impact
* No Downtime
* No over-provisioning

### Steps

Step1 modify
Step2 Monitor
```aws cli

aws ec2 describe-volumes-modifications
[--volumn-ids <value> ]
[--filters <value>]
[--next-token <value>]
[--max-results <value>]

```

The status will show
"Modifying"--> "optimizing"-->"Completed"

Step3 Extend (if size is increased)

```shell
lsblk
df -h
# ext2, ext3, ext4
sudo resize2fs device_name
# xfs
sudo xfs_growfs -d mount_point

```

On widows , run diskmgmt.msc and choose to extend volumns .

__Limitation__:
* max 1 change every 6 hours
* once modification request raise, you can't stop it.
* you can't change the encryption mode for existing volumn  

### Bill

You are charged at the moment you trigger the request with the target volumn.

### Best practise

* Security: lock down the modify-volumn api , treat it as the same as delete volumn. 
* Test Test Test before make any change.

* Use GP2 as boot volumn
* Tag your EBS snapshot (always)
* Your EC2 CPU / Memory should match provisioned EBS
* SSD EBS read / write didn't different in performance
* use Cloudwatch to  tuning EBS
* Raid 0 (don't use Raid 5 , or any raid with redundency)
* Pre-warm : DD write for linux and  NTFS Format; use big block to do pre-warm (like 1M)
* try to use ext4 or XFS; alignment = 4k (???)
* Refer to the cheetsheet for common use case


# References

> Dynamic EBS (2016)
https://youtu.be/9vhR41YTp7E

> aws elastic volumns
https://github.com/aws-samples/aws-elastic-volumes

> EBS Deep Dive (2015)    
https://youtu.be/gUYa7RzrNhM
