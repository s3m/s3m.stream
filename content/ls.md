+++
title = "ls"
template = "page.html"
date = 2022-04-03T11:39:03Z
[taxonomies]
tags = ["list", "ls"]
[extra]
summary = "list objects"
+++


To list the content of a bucket:

    s3m ls <s3 provider>/<bucket>

To list available buckets:

    s3m ls <s3 provider>

To list in-progress multipart uploads:

    s3m ls <s3 provider>/<bucket> -m