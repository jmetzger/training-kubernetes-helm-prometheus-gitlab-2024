# Top 3: Working with environment 

## Schritt 1: Environment anlegen

  * environments für production und development anlegen 

![image](https://github.com/user-attachments/assets/c46a3f9e-e958-47b8-baf2-38d9ff38fb80)
![image](https://github.com/user-attachments/assets/33e1ea7e-ef5c-4b17-85d0-9d35ba353d75)

## Schritt 2: Variablen für ein Environment festlegen 

  * Variable; TEST_URL für production und developement anlegen

![image](https://github.com/user-attachments/assets/3045ef0e-6155-4275-ab05-061ac5f06448)

## Schritt 3: Pipeline editor 

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
     #- for MYVAR in $(cat /tmp/VARLIST); do echo 'export SECRET_'$MYVAR'="'$(eval echo "\$$MYVAR")'"' >> /tmp/MYENV; done 
     - source /tmp/MYENV
     - cat /tmp/MYENV
     - env | grep ^SECRET

build-stage-production:
  environment: production
  stage: build
  extends: .prep
  script: 
     - export KUBECONFIG=$KUBECONFIG_FILE
     - kubectl cluster-info 

build-stage-development:
  environment: development
  stage: build
  extends: .prep
  script: 
     - export KUBECONFIG=$KUBECONFIG_FILE
     - kubectl cluster-info 

```     
