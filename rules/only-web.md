# Nur händisch when man auf pipelines klickt 

```
# gitlab-ci.yml
stages:          # List of stages for jobs, and their order of execution
  - build
  - test 
 
build-job:       # This job runs in the build stage, which runs first.
  stage: build
  only:
    - web
  image: maven
  script:
    - echo "Compiling the code..."
    - echo "Compile complete."

unit-test-job:   # This job runs in the test stage.
  stage: test    # It only starts when the job in the build stage completes successfully.
  only:
    - web 
  script:
    - echo "Running unit tests... This will take about 60 seconds."
    - sleep 60
    - echo "Code coverage is 90%"



```
