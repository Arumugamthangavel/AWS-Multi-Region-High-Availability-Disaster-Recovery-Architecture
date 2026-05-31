# CDN Setup
Objectives: 
* Host a static image in "S3" 
* deliver it globally through "Amazon CloudFront."

## Creating S3 Bucket
* using aws in mumbai region to create S3 bucket.
* I naming it zentechs.site
* uploaing some random image that i have downloaded.
----
![S3 Image Upload](screenshots/s3-image-upload.png)

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
---
![S3 Public Object](screenshots/image-public%20for%20s3.png)

## Creating CloudFront Distribution
* for Origin Domain: Selected my S3 bucket (zentechs.site)
* created a cloudfront distribution.
---
![CDN CREATION](screenshots/showing%20cdn%20is%20created.png)

* checking with domain name if it is working.
---
![domain name checking](screenshots/cdn-static%20hosting.png)

## Now try with Portfolio 
* adding my portfolio html,css and js on S3 buckect
----
[ screenshot ]
![S3 upload for html](screenshots/s3-html%20and%20other.png)
* configure CloudFront "default root object"
```
Default Root Object
 index.html
```
* now checked with cnd domain if it worked
[screenshot of cdn-html]
![cnd domain check](screenshots/cdn-html%20working.png)
______________________________________________________

### Hence, successfully built a real CDN-backed static website
