<p align="center">
  <img src="https://i.ibb.co/W0N0vr0/white-nextjs-1.png" alt="Neverbland Logo"/>
</p>
<p><a href="#" title="Go to GitHub repo"><img src="https://img.shields.io/static/v1?label=kelvinakinolaajayi&message=blog-starter-ts-devops&color=red&logo=github" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a></p>

# Neverbland's Junior DevOps Challenge Documentation

#### Basic Overview
This project involves building and deploying a statically generated Next.js blog site. The project also includes using Amazon Web Services as the destination for the blog site and making a choice on the best service for the task.

# Contents
- Prerequisites & S3 Bucket Creation
- GitHub Actions Workflow
- AWS Service Considerations

# Prerequisites & S3 Bucket Creation
You will need: 
1. An AWS Account
2. A created S3 Bucket
3. Cloudfront Distributtion

In order to replicate this project you will need have set up an account with AWS. Once you have created an account, you will also need to create an S3 bucket. The steps and example images are shwon below:

- Below you will see that the AWS Region I chose is `EU (London) eu-west-2`. You can choose the other regions available depending on where you live. Also make sure to enable the Access Control Lists (ACL's).
<a href="#" title="Go to GitHub repo"><img src="https://gcdn.pbrd.co/images/VYb7DXhTTyP3.png?o=1" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a>

- Untick the block all public access checkbox, to allow access to the website url.
<a href="#" title="Go to GitHub repo"><img src="https://gcdn.pbrd.co/images/dUC9oOFkFzLC.png?o=1" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a>

- Keep all of the remaining settings unchanged and then at the bottom select create bucket.
<a href="#" title="Go to GitHub repo"><img src="https://gcdn.pbrd.co/images/BnqmC5ofAcUl.png?o=1" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a>

- Once the bucket is created you will find different tabs, that will also be of use such as the objects, properties and permissions tab.
<a href="#" title="Go to GitHub repo"><img src="https://gcdn.pbrd.co/images/FsuysTiTrs1J.png?o=1" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a>

- By heading to the properties tab and going to the bottom is where you will find the link to your static site.
<a href="#" title="Go to GitHub repo"><img src="https://gcdn.pbrd.co/images/5EDIjsMpb3PB.png?o=1" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a>

- Next select the permissions tab after clicking on your newly created S3 bucket. To allow for access to the objects in the bucket, include the bucket policy shown in the image below and be sure to include your bucket name for instance `"Resource": "arn:aws:s3:::your-chosen-bucket-name/*"`
<a href="#" title="Go to GitHub repo"><img src="https://gcdn.pbrd.co/images/du7rfRUlXTXK.png?o=1" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a>

- Also within the S3 bucket permissions tab we want to edit the Access Control List and allow public access to everyone by ticking <strong>list</strong> and <strong>read</strong> in the Everyone (public access) column. Once this is done select the save changes button.
<a href="#" title="Go to GitHub repo"><img src="https://gcdn.pbrd.co/images/sRrtmQkccWGT.png?o=1" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a>

- Next we want to use Cloudfront to invalidate our cache and reduce our load on the S3 bucket. This will allow remove a file before it expires. Once we have accessed Cloudfront we create a new distribution and choose the domain name that corresponds with our S3 bucket. In my case this would be `junior-devops-challenge.s3.amazonaws.com`.
<a href="#" title="Go to GitHub repo"><img src="https://gcdn.pbrd.co/images/uG2XWHPamwps.png?o=1" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a>

- Once that is done all settings remain the same and we make sure that the root object is our index.html file and save changes. Now we go to the general tab and distribution we can see the website live when we click the link that says <strong>Distribution domain name</strong>.
<a href="#" title="Go to GitHub repo"><img src="https://gcdn.pbrd.co/images/tjDriBcaKuoZ.png?o=1" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a>

- Lastly we will invalidate our cache by going to the invalidations tab and enter `/*` and then save changes. In the next section I show the GitHub Actions Workflow file that allow us to build and deploy this static site to the s3 bucket once it has been forked.

# GitHub Actions Workflow
```yaml
name: Static Site S3 Deploy

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12.22.0 # The repo uses Tailwind CSS. Tailwind only supports Node.js 12.13.0 or higher
      - run: npm install -g yarn
      - run: yarn install --frozen-lockfile
      - run: yarn build # This command will build the app for production
      - run: yarn next export # This command will export the next.js site
      # The following allows for the AWS account to be validate using GitHub Secrets
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: eu-west-2
          # This last command will make a copy of files that from current directory to s3.
      - run: aws s3 sync ./out s3://junior-devops-challenge --acl public-read
```
The link to the YAML file can be located [here](https://github.com/kelvinakinolaajayi/blog-starter-ts-devops/blob/main/.github/workflows/deploy.yml)

## AWS Service Considerations
When it comes to deciding on a service to deploy a site to AWS, there are many options which I've listed below:
1. AWS Amplify
2. AWS S3 & Cloudfront
3. AWS Elastic Beanstalk
4. AWS Elastic Compute Cloud
5. AWS Lightsail

I've made the choice based on the following considerations:
- Whether the site is static or dynamic
- The pricing of the service
- The collection of conding languages

The other options were not chosen for reasons such as the process being to complicated, the importance placed on the underlying operation system or the app relying on languages such as Ruby or PHP.

Because the website is a static site, the best options for a static site were AWS Amplify or AWS S3 & Cloudront. Because I am dealing with a basic site, I have chosen to go for S3 & Cloudfront due to the simplicity of setup and the most cost effective for the basic site. If the site was more complex then I would go with AWS Amplify due to it having a wide range of features.

#### Why did I use Cloudfront with S3?
Cloudfront caches content in edge locations. So if a user makes a request, the request for the same content will be made but at the location closest to the user. This is more cost effective than serving files straight from S3. There are also no data transfer fees from S3 to Cloudfront. Also Cloudfront allows us to invalidate our cache, so files can be removed before they expire. Below you will find links to the original repo, the workflow and the deployed site itself.

#### Important Links
<p><a href="https://github.com/Neverbland/blog-starter-ts-devops">Original Repo & Task</a></p>
<p><a href="https://d3dtk96o2senbh.cloudfront.net">Deployed Site</a></p>
<p><a href="https://github.com/kelvinakinolaajayi/blog-starter-ts-devops/actions/runs/1582409569/workflow">GitHub Actions Workflow File</a></p>
