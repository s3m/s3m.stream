+++
title = "config.yml"
template = "page.html"
date = 2022-04-03T11:44:30Z
[taxonomies]
tags = ["how", "buffer", "config"]
+++

By default **s3m** will try to use the file located at:

The format of the file is:

    ---
    hosts:
      provider_name:
        region: <region>
        endpoint: <https://endpoint>
        access_key: <access_key>
        secret_key: <secret_key>

For example if you want to use **AWS** S3 and **Backblaze**:

```yaml
---
hosts:
  aws:
    region: eu-central-1
    access_key: ACCESSKEY
    secret_key: SECRETKEY

  backblaze:
    endpoint: s3.us-west-001.backblazeb2.com
    access_key: ACCESSKEY
    secret_key: SECRETKEY

  backup:
    region: us-east-2
    access_key: ACCESSKEY
    secret_key: SECRETKEY
```

> Notice the use of `endpoint` when there is no `region`

From the example, If neeed to save a file to the defined host `backup`:

```sh
s3m /path/to/your/file backup/backups/test.txt
                        |     |      |
                        |     |      text.txt will be the name of the file
                        |     Bucket name: backups
                        backup: defined in config.yml, uses AWS in region us-east-2
```

The bucket `backups` should exist and the provided credentials should have rights to `PUT` objects

Example of an AWS IAM user policy to allow all actions to specific bucket:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::mybucket/*",
                "arn:aws:s3:::mybucket"
            ]
        }
    ]
}
```