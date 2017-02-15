<!-- 
.. title: Delete old EBS snaphot
.. slug: delete-old-ebs-snaphot
.. date: 2017-02-15 13:39:53 UTC+07:00
.. tags: 
.. category: blogs
.. link: 
.. description: 
.. type: text
-->

First, you need to configure your aws credentials

<!-- TEASER_END -->
```
aws configure --profile <your-profile-name>
AWS Access Key ID [None]: xxxxxxx
AWS Secret Access Key [None]: xxxxxx
Default region name [None]: xxxxxx
Default output format [None]: xxxxx
```

Next step will be selecting the just-created aws profile:

```
export AWS_PROFILE=<your-profile-name>
```

Then we're ready to proceed:

```
aws ec2 describe-snapshots --region <region> --filters Name=volume-id,Values=<volume-id> | grep snap- | awk '{print $2}' | awk -F"\"" '{print $2}' | xargs -n 1 -t aws ec2 delete-snapshot --region <region> --snapshot-id
```

Notes:

`<region>`: your aws region (us-east-1 by default)

`<volume-id>`: a volume-id which was used to create snapshot
