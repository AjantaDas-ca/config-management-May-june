### #######################
### AWS Fundamentals
### #######################


### AWS Global Infrastrcture
- Regions | Cluster of 3 or More Availability Zones
- Availability Zones | Cluster of multiple Data Centers
- Edge Locations | CDN | web content caching | CloudFront
- AWS Backbone Network | High Speed low latency cables connecting all AWS DCs

  
- Region
    --> AZs
       --> Data Centres

### EC2

    AMI ->
      VMware Templates

    Amazon Linux ~= CentOS

    m5.xlarge
    - m       --> Family
    - 5       --> Generation
    - xlarge  --> size (vCPU / Memory / Network Bandwidth)

**Firewalls**

- Security Groups
- NACLs (Network ACLs)

### 3 ways to connect to AWS:
- Console (GUI) - user id + Password
- AWS CLI (Command line) - Access key + Secret access key
- SDK | Programmatic way - Access key + Secret access key
### Storage on AWS
- Object (C:\Mymovies)
    - S3 (Buckets ~= Folder/Directory)
- Block (C:\, D:\)
    - EBS (Elastic Block Store)
        - SSD Based
        - HDD Based
- File/Network
    - EFS | Linux Workloads | NFS 4.0 and 4.1
    - FSx | Windows Workloads | NTFS, CIFS, SMB


### Addtional Resources
- https://aws.amazon.com/ec2/instance-types/
- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html
- https://general-webapp.workshop.aws/
- https://www.aws.training/
- https://aws.amazon.com/training/ramp-up-guides/
- 



