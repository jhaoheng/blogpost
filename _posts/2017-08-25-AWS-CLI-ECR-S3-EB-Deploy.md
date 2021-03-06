---
layout: post
title: '【AWS CLI】ECR & S3 & EB deploy'
date: 2017-08-25 17:11
comments: true
categories: AWS
---
## 建立 aws docker image repo
1. 建立 ECR 
	1. 開啟 AWS console -> 選擇 Amazon ECS 的服務 -> 選擇 Repositories
	2. 選擇 create repository
	3. 建立完成後，會看到上傳的方法
2. 按照 ecs 中，的說明 aws ecr get-login 後，會產生出 docker login ... 的語法，透過這個進行 docker login
	1. `aws ecr get-login --no-include-email --region us-east-1` : 取得 `docker login -u AWS -p .... `
	2. 依據 1 中取得的 login 指令，進行 docker login
3. 上傳 docker image 到 ECR 中
	1. `docker build -t xxx {image}`
	2. `docker tag xxx your-ecr-position/your-repo-name:latest`
	3. `docker push your-ecr-position/your-repo-name:latest`

## 透過 beanstalk 部署

0. 建立好一個 eb application
1. 設定好 `Dockerrun.aws.json`
	- `envsubst` 可善用此指令
2. 壓縮 含有設定檔的 folder，並上傳到 aws s3
	- `cd folder && zip -r $folder.zip ./ && aws s3 cp $folder.zip s3://{bucket_name}/{folder.zip}`
3. 建立 eb 新的版本
```
aws elasticbeanstalk create-application-version \
--application-name {your-eb-application-name} \
  --version-label {your-eb-version-label}\
  --source-bundle S3Bucket={bucket_name},S3Key={folder.zip}
```
4. 部署新版本
```
aws elasticbeanstalk update-environment \
--environment-name {your-eb-application-name} \
  --version-label {your-eb-version-label}
```