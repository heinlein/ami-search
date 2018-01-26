# ami-search
Find the latest 64-bit HVM- and EBS-enabled Amazon Machine Images
(AMI) available for certain OS distributions. For most distributions,
we look for gp2 block devices. Images are returned in reverse
chronological order, the most recent being the first listed.

## Rationale

When I launch AWS EC2 instances for myself or for clients, I typically
fall back on just a few OS/Distribution options: Amazon Linux,
CentOS, Ubuntu LTS, or FreeBSD. I like to use the newest AMI
available, so I hacked up this script to list the newest images for
those distributions.

## Prerequisites

This is a bash script, so you'll need bash installed.

More importantly, you'll need the [AWS Command Line
Interface](https://aws.amazon.com/cli/) installed and configured.
`aws` is a Python program, so you'll need Python installed to run
it.  Most mainstream package managers have access to an `awscli`
package.  If not, you can install it via Python's `pip` utility.

## AWS CLI Configuration

Unless you specify a different AWS profile, the "default" one will
be used. Likewise, if you don't specify a region, the region
associated with your default profile will be used.

## Help Screen

```nohighlight
Queries AWS for the latest 64-bit HVM- and EBS-enabled images available
for the specified OS distribution. For most distributions, we look for
gp2 block devices. Images are returned in reverse chronological order,
the most recent being the first listed.

usage: ami-search -d distro [-n num] [-p profile] [-r region] [-s]

-d specifies distribution; currently valid arguments: amazon, amazon2,
   centos6, centos7, freebsd11, or ubuntu1604 [REQUIRED]
-n the maximum number of results to list. Default is 4.
-p specifies a profile in your ~/.aws/credentials file. Uses the
   default profile if none is specified.
-r specifies an AWS region name. The default value for the profile
   you use is found in your ~/.aws/config file.
-s will return only AMI image id. Default is to return image id,
   creation date, and image description.
```

## Examples

These examples are from January 2018. The AMI ID numbers are
region-specific and change over time, so you can expect to see
different IDs when you run the script.

```nohighlight
[~]$ ./ami-search -d centos7
ami-02c71d7a	2017-12-05T03:12:47.000Z	CentOS Linux 7 x86_64 HVM EBS 1708_11.01
ami-51076231	2017-05-09T23:44:25.000Z	CentOS Linux 7 x86_64 HVM EBS 1704_01
ami-0c2aba6c	2017-04-12T00:26:24.000Z	CentOS Linux 7 x86_64 HVM EBS 1703_01
```

```nohighlight
[~]$ ./ami-search -d centos7 -n 1 -s
ami-02c71d7a
```

```nohighlight
[~]$ ./ami-search -d centos7 -r us-east-1
ami-95096eef	2017-12-04T17:19:13.000Z	CentOS Linux 7 x86_64 HVM EBS 1708_11.01
ami-d52f5bc3	2017-05-09T08:55:21.000Z	CentOS Linux 7 x86_64 HVM EBS 1704_01
ami-ae7bfdb8	2017-04-03T20:30:19.000Z	CentOS Linux 7 x86_64 HVM EBS 1703_01
ami-f71ac3e1	2017-03-01T17:45:48.000Z	CentOS Linux 7 x86_64 HVM EBS 1702_01
```

