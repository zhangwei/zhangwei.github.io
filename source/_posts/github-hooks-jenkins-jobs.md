title: github hooks jenkins jobs
date: 2015-09-15 14:56:11
tags:
 - jenkins
 - github
 - webhook
---

### 1. 在github.com的 settings 里追加 webhook

- #### Payload URL: 
```
https://user:api-token@jenkins-server/job/jobname/buildWithParameters?token=asecuretoken
```
- #### Content type:
```
x-www-form-urlencoded  (json option not working...)
```

### 2. 在 jenkins-server 设置 job 

- #### Parameterized Build
```
  String: payload
```
- #### Execut shell
```
#!/bin/bash
echo "PUSHED=`echo $payload|/usr/bin/jq -r '.ref'|sed "s/refs\/heads\///"`" > temp.txt
```


- #### Inject environment variables
```
 	Properties File Path: ${WORKSPACE}/temp.txt
```

- #### Conditional steps (multiple)
```
Run? String match
String1: ${ENV,var="PUSHED"}
String2: master

Steps to run if condition is met
    Trigger/call builds on other projects
    projects-name
```