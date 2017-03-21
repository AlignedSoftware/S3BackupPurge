![alt text](https://github.com/AlignedSoftware/S3BackupPurge/blob/master/images/alignedsoftware.jpg "AlignedSoftware Inc. © 2017")
# S3 Backup Purge 

Purge files from backup buckets, preserving 1 per hour, 1 per day, 1
per week, 1 per month, 1 per year (for 7 years).

Requires python 2.7.X.

## Details

Given a series of files like this, when run on 2017-03-20
```
s3://some-bucket/dir1/dir1-backup-2016-12-31.zip
s3://some-bucket/dir1/dir1-backup-2017-01-01.zip
s3://some-bucket/dir1/dir1-backup-2017-01-02.zip
s3://some-bucket/dir1/dir1-backup-2017-01-03.zip
s3://some-bucket/dir1/dir1-backup-2017-01-04.zip
s3://some-bucket/dir1/dir1-backup-2017-01-05.zip
s3://some-bucket/dir1/dir1-backup-2017-01-06.zip
s3://some-bucket/dir1/dir1-backup-2017-01-07.zip
s3://some-bucket/dir1/dir1-backup-2017-01-08.zip
s3://some-bucket/dir1/dir1-backup-2017-01-09.zip
s3://some-bucket/dir1/dir1-backup-2017-01-10.zip
s3://some-bucket/dir1/dir1-backup-2017-02-01.zip
s3://some-bucket/dir1/dir1-backup-2017-03-01.zip
s3://some-bucket/dir1/dir1-backup-2017-03-02.zip
s3://some-bucket/dir1/dir1-backup-2017-03-18.zip
s3://some-bucket/dir1/dir1-backup-2017-03-19.zip
s3://some-bucket/dir1/dir1-backup-2017-03-20.zip
```

It will reduce the files to 
```
s3://some-bucket/dir1/dir1-backup-2016-12-31.zip
s3://some-bucket/dir1/dir1-backup-2017-01-10.zip
s3://some-bucket/dir1/dir1-backup-2017-02-01.zip
s3://some-bucket/dir1/dir1-backup-2017-03-01.zip
s3://some-bucket/dir1/dir1-backup-2017-03-02.zip
s3://some-bucket/dir1/dir1-backup-2017-03-18.zip
s3://some-bucket/dir1/dir1-backup-2017-03-19.zip
s3://some-bucket/dir1/dir1-backup-2017-03-20.zip
```

It also, in the output, specifies *why* it is deleting a file, such as:

```
Deleteing 's3://some-bucket/dir1/dir1-backup-2017-01-09.zip' because we have 's3://some-bucket/dir1/dir1-backup-2017-01-10.zip' as monthly backup
```

## Files Impacted and Files Ignored

This script will *only* impact files with an [ISO 8601](https://www.w3.org/TR/NOTE-datetime)
format such as:

'[^/]+/[a-zA-Z0-9]+-YYYY-MM-DD(THH:MM:SS)\..*' (where Y,M,D,H,M,S are
all digits and expected to provide a valid date and/or/time) and purge
them according to the algorithm.  

Any files with date but missing time will be assumed to be created at
midnight (time=0) on that day.

Any files that does *NOT* match this pattern will be ignored and
preserved in S3.


## Example Usage

Will perform the purge against the AWS bucket testing-s3purge and print reasons.
Assumes a .boto file filled in properly with AWS creds in your home directory.
```shell
$ ./s3purge -bucket=testing-s3purge
```

Will *not* perform the purge against the AWS bucket testing-s3purge but will print reasons.
It's a dry-run. Just like make.
```shell
$ ./s3purge -bucket=testing-s3purge -n
```

Will perform the purge against the AWS bucket testing-s3purge and print reasons and debugging information.
```shell
$ ./s3purge -bucket=testing-s3purge -verbose
```

Will perform the purge against the AWS bucket testing-s3purge and print reasons and debugging information
with credentials within the profile 
```shell
$ ./s3purge -bucket=testing-s3purge -verbose -profile=/path/to/my/botofile
```

For the help:
```shell
$ ./s3purge -h
$ ./s3purge --help
```

## .boto Profile

An example of a .boto profile for AWS would be the following:

```
[Credentials]
aws_access_key_id =ABCDEFGHIJKLMNOP
aws_secret_access_key=aHLLmlLJ678+jljl9779734

```
