+++
title = "ACL"
template = "page.html"
date = 2022-04-03T11:37:03Z
[taxonomies]
tags = ["acl", "private", "xml"]
[extra]
summary = "PUT or GET object ACL"
+++


If need to set or get an ACL without uplaoding a file you can use the subcomand
`acl`, for example to make an object `private`:

```sh
s3m acl <s3 provider>/<bucket>/object -a private
```

To retrive the existing `ACL`:

```sh
s3m acl <s3 provider>/<bucket>/object  | xmllint --format -
```
> notice the usas for xmlllint to pretty print the output