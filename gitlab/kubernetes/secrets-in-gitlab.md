# Walkthrough: Secrets in gitlab ci/cd 

## Schritt 1: 

```
Variablen in SETTINGS--> CI/CD anlegen
```
![image](https://github.com/user-attachments/assets/c1b94f5f-7b10-40a4-a41f-dd653568f46f)

## Schritt 2: Beispiel - Secret in manifests/mysecret.yaml 

  * SECRETS als Variablen mit Prefix SECRET
  * in gitlab ci/cd ohne PREFIX gespeichert  

```
apiVersion: v1
data:
  TEST_CONTENT: $SECRET_TEST_CONTENT
  TEST_URL: $SECRET_TEST_URL
kind: Secret
metadata:
  creationTimestamp: null
  name: mysecret

```

## Schritt 3: pipeline - script 

```
default:
  image: alpine

stages:
  - build 

build-stage:
  stage: build
  script: 
     - apk add kubectl envsubst
     - env | grep ^TEST
     - for i in $(env | grep ^TEST); do MYVAR=$(echo $i | cut -d '=' -f 1); if [ ${MYVAR:0:4} == "TEST" ]; then echo $MYVAR >> /tmp/VARLIST; fi; done 
     - cat /tmp/VARLIST
     - for MYVAR in $(cat /tmp/VARLIST); do echo 'export SECRET_'$MYVAR'="'$(eval echo "\$$MYVAR" | base64)'"' >> /tmp/MYENV; done 
     - source /tmp/MYENV
     - cat /tmp/MYENV
     - env | grep ^SECRET
     # Now we get a file 
     - cat manifests/mysecret.yaml | envsubst > /tmp/mysecret.yaml
     - cat /tmp/mysecret.yaml
```


## Schritt 4: try and feel happy 
