+++
title = "upload"
template = "page.html"
date = 2022-04-03T11:40:03Z
[taxonomies]
tags = ["put", "pipe", "acl", "meta"]
[extra]
summary = "put objects"
+++


To upload a file to an s3 bucket:

```sh
s3m /path_to/file <s3 provider>/<bucket>/file
```

For example if using this `~/.s3m/config.yml`:

```yaml
---
hosts:
  aws:
    region: eu-central-1
    access_key: ACCESSKEY
    secret_key: SECRETKEY
```

To upload a file to `aws` to a bucket named `backup` you could do:

```sh
s3m /path/to/file aws/backup/file
```
> `aws` is the S3 provider, `backup` is the bucket


## PIPE/STDIN

You could pipe the output of your application or a file for example:

```sh
mysqldump | xz -c | s3m --pipe <s3 provider>/<bucket>/dump.sql
```

> The `s3 provider` could be `aws` from the previous example.


### ACL

When streaming, a [canned ACL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/acl-overview.html#canned-acl)
for the object can be defined if using option `-a/--acl <acl>`, for example to make the object public:

```sh
s3m /path_to/file <s3 provider>/<bucket>/file -a public-read
```

### Metadata

To add a custom, user-defined object metadata `x-amz-meta-*`, the option `-m/--meta` can be used, for example:

```sh
s3m /path_to/file <s3 provider>/<bucket>/file -m "key1=value1;key2=value2"
```

This will create the object with the following metadata:

```yaml
x-amz-meta-key1: value1
x-amz-meta-key2: value2
```