+++
title = "rm"
template = "page.html"
date = 2022-04-03T11:38:03Z
[taxonomies]
tags = ["rm", "delete"]
[extra]
summary = "delete objects"
+++


To remove an object:

    s3m rm <s3 provider>/<bucket>/object

To abort a multipart upload:

```sh
s3m rm <s3 provider>/<bucket> -a <UploadId>
```

> to get the upload ID you could use `s3m ls <s3 provider>/<bucket> -m`

For example to abort all multipart uploads:

```sh
s3m ls -m <s3>/<bucket> | awk '{system("s3m rm <s3>/<bucket>/"$5" -a "$4);}'
```