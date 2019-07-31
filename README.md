### Copy S3 bucket objects across AWS accounts.

Copy s3 bucket *pl.kamil.kurek* to another aws account bucket *pl.kamil.kurek.migrated*.

##### Attach policy to source bucket.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DelegateS3Access",
            "Effect": "Allow",
            "Principal": {"AWS": "268868089671"},
            "Action": ["s3:ListBucket","s3:GetObject"],
            "Resource": [
                "arn:aws:s3:::pl.kamil.kurek/*",
                "arn:aws:s3:::pl.kamil.kurek"
            ]
        }
    ]
}
```

##### Attach policy to IAM user in destination AWS account

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::pl.kamil.kurek",
                "arn:aws:s3:::pl.kamil.kurek/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": [
                "arn:aws:s3:::pl.kamil.kurek.migrated",
                "arn:aws:s3:::pl.kamil.kurek.migrated/*"
            ]
        }
    ]
}
```

##### Sync s3 objects to destination

```bash
aws s3 sync s3://pl.kamil.kurek s3://pl.kamil.kurek.migrated
```
