# Use shell - script 

## Add good.sh as script in repo 

```
# in repository top-level
good.sh
```

```
#!/bin/bash 

echo "hello good" 
exit 0
```

## Adjust pipeline 

```
stages:
  - build 

workflow:
  rules:
  - if: $CI_PIPELINE_SOURCE == "web"

build-stage:
  stage: build
  variables:
    CMD: |
      echo "best bet";
      echo hello-you; 
      ls -la;
  script: 
  - bash -s < good.sh
  - echo -n $CMD  
```
