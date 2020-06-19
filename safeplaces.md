<div align="center">
    <img src="./imgs/logo-dark.png">
</div>
<h1>
    SafePlaces Documentation
</h1>
<h3>
    Covi-ID is an open source risk management tool designed to protect privacy.
</h3>

---
## Frontend
- Hosted on AWS S3 Static Site
- The repo is forked from here [here](https://github.com/Path-Check/safeplaces-frontend)

### How to deploy
1. Make sure the latest code you want to push is on `dev` branch
2. Then go to AWS CodePipeline and release change on coviid-safeplaces-frontend-dev
3. During the build process the code latest code is pulled from our fork version dev , builds it and get the correct .env  from the respective s3 bucket and pushes to AWS S3 bucket where the web app is hosted

## Backend
- Host on AWS Elastic Beanstalk on a docker container
- The repo is forked from [here](https://github.com/Path-Check/safeplaces-backend)
- The LDAP server is setup on the same server that the api is running on
- A PostGres database is hosted on AWS RDS

### How to deploy
1. Make sure the latest code you want to push is on dev branch
2. Then got to AWS CodePipeline and release change on coviid-safeplaces-backend-dev
3. During the build process the code latest code is pulled from our fork version `dev` , builds it and get the correct `.env`  from the respective s3 bucket and pushes the latest code to ELB which then builds and runs the docker image

### Setup an LDAP server
1. SSH into ELB instance
2. Connect to instance and install LDAP on the instance as root. Use [this](https://www.itskarma.wtf/openldap-on-ec2/) reference
3. Create a user admin user that matches the admin users seeded to the database by creating a `.ldif` in a `/tmp` directory, here’s a sample
```
# For adding an admin user to the LDAP directory
dn: cn=username,dc=safeplaces,dc=africa
cn: username
sn: username
objectClass: person
userPassword: password
```