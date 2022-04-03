+++
title = "Example"
template = "page.html"
date = 2022-04-03T11:45:10Z
[taxonomies]
tags = ["how", "buffer", "config"]
[extra]
summary = "How it works"
+++

To stream a file simply do:

```sh
s3m /path/to/your/file aws/bucket/destination
                        |     |      |
                        |     |      Path to store the file
                        |     Bucket
                        Host (S3 provider)
```


For that to work you need to previously configure **s3m** `config.yml` or pass the
configuration file using option `-c/--config`

## The config.yml

The default location is `$HOME/.s3m/config.yml`

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
```

> Notice the use of `endpoint` when there is no `region`


## How it works:
**s3m** upload the files in different ways:

* If the input file is **<** than the buffer size, the file will be uploaded in one-show (not capable of resuming).
* if the input file is **>** than the buffer size, the file will be uploaded in multi parts and in it can be resumed.

> The buffer size by default is 10MB and it can be changed using the option `-b`.

**s3m** calculates the buffer size automatically, but if required and if you
know already the file size, you can choose a buffer size, based on your needs.

For example, if you would like to upload 5TB (the current max object size) you
will need a buffer size of **512MB**:

```sh
s3m /path_to/5TB.file <s3>/<bucket>/file -b 536870912
```

If using buffer of **30MB** you could upload up to 300GB:

```sh
s3m /path_to_max/300GB.file <s3>/<bucket>/file -b 31457280
```

The current [limits for AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/qfacts.html) are:

- Maximum object size: 5 TB
- Maximum number of parts per upload:  10,000
- Part size: 5 MB to 5 GB

If you would like to upload up to 500GB objects:

```
524288000000     / 10000  = 52428800 (50MB buffer size)
(500GB in bytes) / (max number of parts)
```

The buffer size to use **50MB**:

```sh
s3m /path_to_max/500GB.file <s3>/<bucket>/file -b 52428800
```

> As mentioned, this is optional since **s3m** calculates this automatically

When the size of the input is known in advance, a checksum of the file is
created before unloading the file in order to keep track of the uploaded
objects, this helps to prevent uploading the same file twice besides "keeping
state" when uploading in multi parts so that if the upload gets interrupted it
can be resumed later.

To clean up the local database you could use the option `--clean`, for example:

```sh
s3m --clean
```

Currently, there is no checksum when piping (option `-p/--pipe`) / sending the
file via STDIN.