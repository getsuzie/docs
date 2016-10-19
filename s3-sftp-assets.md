# S3/SFTP Assets

Instead of storing assets/media (uploaded through the WordPress dashboard) on a single server or in Git, Suzie will upload them to either S3 or SFTP. At first the asset will be uploaded to the server that's in use, then it will be moved to remote storage.

## Amazon S3

Let's go through setting up a bucket to use with Suzie. For this example I'll set one up for to use with `bigbitecreative.com`. First sign up to [AWS](http://aws.amazon.com) and go to the S3 section:

![alt text](http://getsuzie.com/assets/images/docs/s3-1.png "S3-1")

Select `Create Bucket`:

![alt text](http://getsuzie.com/assets/images/docs/s3-2.png "S3-2")

Lets enter `assets.bigbitecreative.com` and I'll select Ireland. And click `Create`. Once created click on the bucket properties and select `Edit bucket policy`

![alt text](http://getsuzie.com/assets/images/docs/s3-3.png "S3-3")

Now we need to give everyone read access. Paste the following in, replacing `bucket.name`:

```
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"AddPerm",
      "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::bucket.name/*"]
    }
  ]
}
```
Now click `Save`. Just like you did with `Edit bucket policy`, select `Add CORS Configuration` and paste the following in:

```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>Authorization</AllowedHeader>
        <AllowedHeader>Content-*</AllowedHeader>
        <AllowedHeader>Host</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```
Now the last thing you will need is your access details to allow WordPress to upload files. At the top of window click on your name (for me `Jason Agnew`) then `Security Credentials`. On the left of the new screen select `Users`:

![alt text](http://getsuzie.com/assets/images/docs/s3-5.png "S3-5")

Select `Create New Users`:

![alt text](http://getsuzie.com/assets/images/docs/s3-6.png "S3-6")

Lets enter `WordPress-BigBite` and select `Create`:

![alt text](http://getsuzie.com/assets/images/docs/s3-7.png "S3-7")

Now take a copy of your `Access Key ID` and `Secret Access Key` and click `Close`

![alt text](http://getsuzie.com/assets/images/docs/s3-8.png "S3-8")

It's time to give that user access to your S3 bucket. Click `Attach Policy`

![alt text](http://getsuzie.com/assets/images/docs/s3-9.png "S3-9")

Select `AmazonS3FullAccess` then `Attach Policy`. It's important to note this gives this user access to all buckets so you may which to refine this later. Now we have all the details we need to set up our `.env`

```
SYNC=s3

S3_KEY=XXXXXXXXX
S3_SECRET=xxxxxxxxxxxxxxxxx
S3_REGION=eu-west-1
S3_BUCKET=assets.bigbitecreative.com
S3_URL=https://s3-eu-west-1.amazonaws.com/assets.bigbitecreative.com/
```

Now if you want the S3_URL to be `http://assets.bigbitecreative.com` you can add CNAME to your DNS:

`CNAME assets.bigbitecreative.com assets.bigbitecreative.com.s3-eu-west-1.amazonaws.com`

## SFTP

Suzie also supports SFTP for storing assets:

```
SYNC=sftp

SFTP_HOST=222.111.144.144
SFTP_PORT=22
SFTP_USER=root
SFTP_PASSWORD=password
SFTP_PATH=/path/to/storage/
SFTP_URL=http://domain.com/path/
```
