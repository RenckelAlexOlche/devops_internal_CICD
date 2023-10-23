# CI CD Project

![CI CD Architecture](images/ci-cd-architecture.png)

## Launching instances

Using Terraform code launch EC2:

Then start the creating process

```
terraform init
terraform apply
```

## Host server configuration (with Jenkins)

First, add add permissions to docker socket (read and write for all)

```
sudo chmod a=rw /var/run/docker.sock
```

Next goto `http://your-host-server-ip:8080/`

For registration, you need to unlock Jenkins using the initial administrator password

```
sudo docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Set english language as default (optional)

- Manage Jenkins >> Manage Plugins >> Available plugins

![locale](images/locale.png)

- Manage Jenkins >> Configure System

![default language](images/default-language.png)

Install docker pipeline

![docker pipeline](images/docker-pipeline.png)

Install ansible plugin to jenkins

- Manage Jenkins >> Manage Plugins >> Available plugins

![install ansible plugin](images/install-ansible-plugin.png)

Need to download ansible to jenkins container

```
docker exec -it --user root jenkins bash

apt-get update

apt-get install -y ansible

Geographic area: 8

Time zone: 24
```

Create hosts file in ansible folder (need for connecting to machine where will target project)

```
cd /etc

mkdir ansible

cd ansible

nano hosts
```

- in 'hosts' file set ip of target server

```
[webserver]
54.156.166.0
```

Add credentials for ssh connection with machine

- Manage Jenkins >> Manage Credentials >> Available plugins

![ansible add new credentials](images/ansible-add-new-credentials.png)

- create ssh credentials

![ansible credentials](images/ansible-credentials.png)

Add docker hub credentials

![docker hub credentials](images/docker-hub-credentials.png)

Add ansible tools for generate pipeline code

- Manage Jenkins >> Global Tool Configuration

![add ansible tools](images/add-ansible-tools.png)

Add github webhook

- configure jenkins API Token

![configure user](images/webhook-configure-user.png)

![generate token](images/webhook-generate-token.png)

- add webhook in github

![create webhook](images/webhook-create.png)

![webhook credentials](images/webhook-credentials.png)

![webhook events](images/webhook-events_1.png)

![webhook events](images/webhook-events.png)


Add pipeline job with ansible

- create new job

![add ansible job](images/add-ansible-job.png)

- add triggers

![triggers](images/triggers-finaltask-job.png)

- pipeline code

![job pipeline](images/job-pipeline.png)

Save job and run it, then the job will start itself after the push to github

![job sone](images/docker-hub-job.png)


### If job won't working, try execute this code in target machine

```
sudo docker exec mysql-server-container mysql -u root --password=31xLobOJO4fFUKE62oOFA8ev1jhFRq -e "create database devops_finaltask_db;"
```





















