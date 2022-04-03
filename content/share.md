+++
title = "share"
template = "page.html"
date = 2022-04-03T11:38:03Z
[taxonomies]
tags = ["share"]
[extra]
summary = "Sharing objects using presigned URLs"
+++


To share an object for 12 hours (`43200` seconds):

    s3m share <s3 provider>/<bucket>/object

The max value is 604800 (7 days)

```sh
s3m share <s3 provider>/<bucket>/object -e 604800
```