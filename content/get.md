+++
title = "download"
template = "page.html"
date = 2022-04-03T11:40:02Z
[taxonomies]
tags = ["get", "meta"]
[extra]
summary = "get objects"
+++


To `get` a file

```sh
s3m get /path_to/file <s3 provider>/<bucket>/file
```

If only need to retrieve the metadata from an object without downloading it, use option `-H`:

```sh
s3m get /path_to/file <s3 provider>/<bucket>/file -H
```