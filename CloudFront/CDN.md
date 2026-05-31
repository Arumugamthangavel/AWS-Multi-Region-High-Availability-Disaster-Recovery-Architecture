# CDN Setup
Objectives: 
* Host a static image in "S3" 
* deliver it globally through "Amazon CloudFront."

## Creating S3 Bucket
* using aws in mumbai region to create S3 bucket.
* naming it zentechs.site
* uploaing some random image that i have downloaded downloaded.
-[screenshot of creation of bucket]

## making object public
Making the bucket public temporarily. To Verify the image opens. Then create CloudFront.
* first, disable block public access and save it
* second, adding a buckect pocily
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::zentechs.site/*"
    }
  ]
}
```
* after that copy the URL and check if it is working
-[screenshot of image-working]
