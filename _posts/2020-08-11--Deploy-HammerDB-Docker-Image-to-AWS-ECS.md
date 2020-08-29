---
title: Deploy a HammerDB Docker Image to AWS ECS
subTitle: HammerDB to AWS ECS
category: [AWS, ECS]
tags: [AWS, HammerDB, Docker, ECS]
cover: AWS_ECS.png
---

## Push the HammerDB Image to Docker Hub
Create a [docker hub account](https://hub.docker.com/signup) and [repository](https://docs.docker.com/docker-hub/repos/#creating-repositories) if you don't have.


Run following command to tag the hammerdb image and push to docker hub a public repository. 

P.S. Please replace my repository with your repository

```
docker tag hammerdb andrewlo1011/hammerdb
docker push andrewlo1011/hammerdb
```
![](/assets/img/20200811/10_push_image_to_docker_hub.jpg)

## Create ECS Cluster
__NOTE: We will create AWS resources that incur costs. It is chargeable, please be aware.__

Create an ECS cluster as below:

![](/assets/img/20200811/20_Create_ECS_cluster.jpg)

Pick the Fargate ECS type, which don't need to maintain our own EC2.
It is kind of serverless ECS.

![](/assets/img/20200811/21_Create_ECS_cluster.jpg)

Enable Container Insights for CPU usage monitoring. 
![](/assets/img/20200811/22_Create_ECS_cluster.jpg)

![](/assets/img/20200811/24_Create_ECS_cluster.jpg)
![](/assets/img/20200811/25_Create_ECS_cluster.jpg)

## Create Task and Container definition

Select Fargate launch type:
![](/assets/img/20200811/26_Create_task_definition.jpg)
![](/assets/img/20200811/40_task_definition.jpg)

Task Size: 2 vCPU and 4G Memory
![](/assets/img/20200811/41_task_definition_b.jpg)

Add the hammerdb Container from docker hub to the definition:
![](/assets/img/20200811/30_add_container_b.jpg)

Add the environment variables for Oracle easy connection string, database username and password: 
![](/assets/img/20200811/31_add_container_b.jpg)

![](/assets/img/20200811/32_add_container.jpg)

Add the awslog configuration, so that we could view the log in CloudWatch.
![](/assets/img/20200811/33_add_container.jpg)

Continue to create task definition:
![](/assets/img/20200811/42_task_definition.jpg)
![](/assets/img/20200811/43_task_definition.jpg)


## Run the hammerdb Task definition

![](/assets/img/20200811/50_run_task.jpg)
![](/assets/img/20200811/51_run_task.jpg)

In the environment variables override, set the Oracle RDS connection string, system username and password.

P.S. 
The step to create the testing RDS Oracle database is not included here.
Also the HammerDB user creation and warehouse build in the database also not included here.

![](/assets/img/20200811/52_run_task.jpg)
![](/assets/img/20200811/53_run_task.jpg)
![](/assets/img/20200811/54_run_task.jpg)
![](/assets/img/20200811/55_run_task.jpg)
![](/assets/img/20200811/56_run_task.jpg)
![](/assets/img/20200811/57_run_task.jpg)

## Round 1 Task Execution Result
The round 1 test was executed with 1, 2, 3 virtual users respectively.
However, after review the database statistic for 3 virtual users, I found that the database loading was a bit low. 
After review, I found that load.tcl has an typo for the 3rd part of the test, it was set to 1 virtual user.

![](/assets/img/20200811/60_run_task_stats.jpg)
![](/assets/img/20200811/61_run_task_stats.jpg)
![](/assets/img/20200811/62_run_task_stats.jpg)
![](/assets/img/20200811/63_run_task_stats.jpg)
![](/assets/img/20200811/64_run_task_stats.jpg)
![](/assets/img/20200811/65_run_task_stats.jpg)
![](/assets/img/20200811/66_run_task_stats.jpg)
![](/assets/img/20200811/67_run_task_stats.jpg)
![](/assets/img/20200811/68_run_task_stats.jpg)

## Round 2 Task Execution Result
I fixed the load.tcl script and increased the virtual users in the script.
I rebuilt the image and push to docker hub again.
The round 2 test was executed with 3, 4, 5 virtual users 
It seems that the current RDS sizing the performance was saturated at 3 virtual users. 

![](/assets/img/20200811/70_run_task_stats.jpg)
![](/assets/img/20200811/71_run_task_stats.jpg)
![](/assets/img/20200811/72_run_task_stats.jpg)
![](/assets/img/20200811/73_run_task_stats.jpg)
![](/assets/img/20200811/74_run_task_stats.jpg)
![](/assets/img/20200811/75_run_task_stats.jpg)
![](/assets/img/20200811/76_run_task_stats.jpg)
![](/assets/img/20200811/77_run_task_stats.jpg)
![](/assets/img/20200811/78_run_task_stats.jpg)
