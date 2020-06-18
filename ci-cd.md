<div align="center">
    <img src="./imgs/logo-dark.png">
</div>
<h1>
    Continuous Integration/ Deployment
</h1>
<h3>
    Covi-ID is an open source risk management tool designed to protect privacy.
</h3>

---
## Frontend

- Hosted on AWS S3 Static Site
- CodePipeline
- CloudFront

### How to deploy
This repository has webhooks setup for deployment on 3 branches, namely:
- develop
- staging
- master
1. Make sure the latest code is on the branch you want to deploy
2. CodePipeline will then automatically build and push the latest updates to the respective bucket
3. Then go to AWS CloudFront and invalidate the cdn that is using the bucket you pushed to, so that you do not see a old or cache version

## Backend

- Hosted on AWS Elastic Beanstalk IIS
- CodePipeline

### How to deploy
This repository has webhooks setup for deployment on 3 branches, namely:
- develop
- release (staging)
- master

1. Make sure the latest code is on the branch you want to deploy
2. CodePipeline will then automatically build and push the latest updates to Elastic Beanstalk
