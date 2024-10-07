# Explore prometheus 

## Step 1: Prerequisites: Port-Forward svc/prometheus-prometheus 9090

  * Eventually you will need to open a ssh-tunnel

## Step 2: Now we explore:

### 2.1. Config 

  * http://localhost:9090/config (Status -> Config)
  * This config will get automatically created by the operator and objects (e.g. ServiceMonitor) 

 ### 2.2. TSDB-Status 

  * Shows the status of the TSDB (Time Series Database)
  * http://localhost:9090/tsdb-status
  * nothing fancy involved here

### 2.3. Command-line flags 

  * With which params was prometheus started
  * Only a bit helpful, because we do not use these flags in most cases
  * http://localhost:9090/flags

### 2.4. Alerting Rules 

  * Defines, when alerts trigger
  * All these are predefined, and the is a great library here:
    * https://runbooks.prometheus-operator.dev/
    * When an alert is fired, you will see this reference 
  * http://localhost:9090/rules

### 2.5. Targets (real targets being scraped)

  * IMPORTANT
  * These are the targets that have been discovered based on
    * ServiceMonitor
    * configuration (ServiceMonitor will always be shown in config-file) after being parsed
   
### 2.6. Service Discovery 

  * Prometheus includeds a mechanism of service discovery
    * which essentially queries the kube-api-server to find out about the services
  * This is also used by the ServiceMonitor, but you can also configure it manually


