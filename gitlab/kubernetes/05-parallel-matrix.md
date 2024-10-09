# Top 5: Execute one job multiple times (parallel matrix) 

## Pipeline Editor (entsprechend ändern) 

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
  environment: $ENVIRONMENT
  stage: build
  extends: .prep
  script: 
     - export KUBECONFIG=$KUBECONFIG_FILE
     - kubectl cluster-info 
     - cd manifests; kubectl apply --dry-run=client -o yaml -R -f . | envsubst > /tmp/all-manifests.yml
     - cat /tmp/all-manifests.yml
     - kubectl apply -f /tmp/all-manifests.yml
     - kubectl get all
  parallel:
    matrix:
      - ENVIRONMENT: ['production','development']

```
