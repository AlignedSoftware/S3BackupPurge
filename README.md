= S3 Backup Purge =

Purge files from backup buckets, preserving 1 per hour, 1 per day, 1
per week, 1 per month, 1 per year (for 7 years).



== Details ==

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
