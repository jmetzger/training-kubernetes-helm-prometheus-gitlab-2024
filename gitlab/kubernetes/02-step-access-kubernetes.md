# Top 2: Access Kubernetes 

## Step 1: Set KUBECONFIG_FILE -> Settings -> CI/CD -> Variables 

   * Use your credentials on client from ~/.kube/config

![image](https://github.com/user-attachments/assets/064b8924-756e-4d3a-a5dd-b7fc4acff737)

## Step 2: Adjust in Pipeline editor:

```
default:
  image: alpine

stages:
  - build 

.prep: 
  before_script:  
     - apk add kubectl envsubst
     - env | grep ^TEST
     - for i in $(env | grep ^TEST); do MYVAR=$(echo $i | cut -d '=' -f 1); if [ ${MYVAR:0:4} == "TEST" ]; then echo $MYVAR >> /tmp/VARLIST; fi; done 
     - cat /tmp/VARLIST
     - for MYVAR in $(cat /tmp/VARLIST); do echo 'export SECRET_'$MYVAR'="'$(eval echo "\$$MYVAR" | base64)'"' >> /tmp/MYENV; done 
     - source /tmp/MYENV
     - cat /tmp/MYENV
     - env | grep ^SECRET

build-stage:
  stage: build
  extends: .prep
  script: 
     - export KUBECONFIG=$KUBECONFIG_FILE
     - kubectl cluster-info 
     
     # Now we get a file 
     # - cat manifests/mysecret.yaml | envsubst > /tmp/mysecret.yaml
     # - cat /tmp/mysecret.yaml

```
