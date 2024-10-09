# Kubernetes, Helm, Prometheus, Gitlab

## Agenda 

  1. Docker-Grundlagen 
     * [Übersicht Architektur](architektur.md)
     * [Was ist ein Container ?](container.md)
     * [Was sind container images](container-images.md) 
     * [Container vs. Virtuelle Maschine](container-vs-vm.md)
     * [Was ist ein Dockerfile](dockerfile.md) 
     * [Dockerfile - image kleinhalten](dockerfile-image-small.md)

  1. Kubernetes - Überblick
     * [Warum Kubernetes, was macht Kubernetes](warum-kubernetes.md)
     * [Aufbau Allgemein](/kubernetes/architecture.md)
     * [Wann macht Kubernetes Sinn, wann nicht?](/kubernetes/wann-sinnvoll-wann-nicht.md)   
     * [Aufbau mit helm,OpenShift,Rancher(RKE),microk8s](/kubernetes/aufbau-helm-microk8s-kubernetes.md)
     * [Welches System ? (minikube, micro8ks etc.)](welches-system.md)
     * [Installation - Welche Komponenten from scratch](/kubernetes/installation-components-overview.md)  
      
  1. kubectl installieren und einrichten
     * [Ubuntu client aufsetzen](/tipps-tricks/ubuntu-client.md)
     * [bash-completion](/kubectl/bash-completion.md) 
     * [kubectl einrichten mit namespace](/kubectl/kubectl-einrichten.md)
     * [kubectl cheatsheet kubernetes](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

  1. Kubernetes Praxis API-Objekte 
     * [Das Tool kubectl (Devs/Ops) - Spickzettel](/kubectl/spickzettel.md)
     * [kubectl example with run](/kubectl/run-with-example.md)
     * [Bauen einer Applikation mit Resource Objekten](bauen-einer-webanwendung.md)
     * [kubectl/manifest/pod](/kubectl-examples/01-pod-nginx.md)
     * ReplicaSets (Theorie) - (Devs/Ops)
     * [kubectl/manifest/replicaset](/kubectl-examples/01a-replicaset-nginx.md)
     * Deployments (Devs/Ops)
     * [kubectl/manifest/deployments](/kubectl-examples/03-nginx-deployment.md)
     * Debugging 
     * [Netzwerkverbindung zum Pod testen](/tipps-tricks/verbindung-zu-pod-testen.md)  
     * Services (Devs/Ops)
     * [kubectl/manifest/service](/kubectl-examples/03b-service.md)
     * DaemonSets (Devs/Ops)
     * IngressController (Devs/Ops)
     * [Hintergrund Ingress](/kubernetes/ingress.md) 
     * [Ingress Controller auf Digitalocean (doks) mit helm installieren](/digitalocean/ingress-auf-digitalocean-mit-helm.md)
     * [Documentation for default ingress nginx](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/configmap/)
     * [Beispiel Ingress](/kubectl-examples/04-ingress-nginx.md)
     * [Beispiel mit Hostnamen](/kubectl-examples/04-ingress-nginx-with-hostnames.md)
     * [Achtung: Ingress mit Helm - annotations](/ingress-mit-helm-class-achtung.md)
     * [Permanente Weiterleitung mit Ingress](/kubectl-examples/05-ingress-permanent-redirect.md)
     * [ConfigMap Example](/kubectl-examples/06-configmap.md)
     * [ConfigMap Example MariaDB](/kubectl-examples/06a-configmap-mariadb.md)

  1. Kubernetes - Jobs
     * [Documentation Jobs + kubectl cp](kubectl-examples/99-jobs.md)
    
  1. Kubernetes Administration / Tipps & Tricks
     * [Hängenden Namespace löschen - Stuck](kubectl/delete-stuck-namespace.md)

  1. LoadBalancer on Premise (metallb)
     * [Metallb](load-balancer/metallb.md)

  1. Kubernetes Storage (CSI) 
     * [Überblick Persistant Volumes (CSI)](kubernetes-csi/overview.md)
     * [Übung Persistant Storage](kubernetes-csi/nfs-exercise.md)
     * [Beispiel mariadb](kubernetes-csi/example-mariadb.md)

  1. Helm (Kubernetes Paketmanager) 
     * [Warum ? (Dev/Ops)](/helm/warum.md)
     * [Grundlagen / Aufbau / Verwendung (Dev/Ops)](/helm/grundlagen.md)
     * [Praktisches Beispiel bitnami/mysql (Dev/Ops)](/helm/example.md) 

  1. Kubernetes Secrets / ConfigMap
     * [Secret with mariadb](kubectl-examples/06b-secret-mariadb.md)
     * [Secrets Example 1](kubernetes/secrets/uebung-secrets.md)
     * [Änderung in ConfigMap erkennen und anwenden](https://github.com/stakater/Reloader)
    
  1. Kubernetes - gitlab ci-cd - Projekt (secrets,kubectl,environments)
     * [01 Kubernetes Secrets in gitlab ci/cd](gitlab/kubernetes/secrets-in-gitlab.md)
     * [02 Access Kubernetes](gitlab/kubernetes/02-step-access-kubernetes.md)
     * [03 Working with environments](gitlab/kubernetes/03-working-with-environments.md)
     * [04 Deploy manifests to cluster](gitlab/kubernetes/04-deploy-to-cluster-md)
     * [05 Execute jobs multiple times - one job definition](gitlab/kubernetes/05-parallel-matrix.md)
  
  1. gitlab ci/cd - Überblick 
     * [gitlab Architektur](gitlab/architecture.md)
     * [Overview/Pipelines](/gitlab/01-ci-cd-overview.md)
    
  1. gitlab ci/cd - Praxis I
     * [Using the test - template](/gitlab/02-example-testtemplate.md)
     * [Examples running stages](/gitlab/03-example-running-stages.md) 
     * [Predefined Vars](/gitlab/04-predefined-vars.md)
     * [Variablen definieren](/gitlab/variables.md)
     * [Example Defining and using artifacts](/gitlab/07-example-defining-and-using-artifacts.md)

  1. gitlab ci/cd (Artifacts)
     * [Pass artifacts between jobs in same stage](gitlab/artifacts/pass-between-jobs-in-same-stage.md)

  1. gitlab ci/cd (Praxis II)
     * [Mehrzeile Kommandos in gitlab ci-cd ausführen](/gitlab/jobs/script/multiline.md)

  1. gitlab ci/cd - Tipps & Kniffe
     * [Warum before_script ?](/gitlab/why-before-script.md)
     * [GIT_STRATEGY usw.](gitlab/git_strategy.md)

 1. gitlab-ci/cd - Workflows
     * [Workflows + only start by starting pipeline](/gitlab/global/workflow.md)
     * [Templates for branch and merge request workflow](workflow/templates-branch-merge-request-pipeline.md)
    
  1. gitlab-ci/cd - Variables
     * [Variablen in Pipelines Web-Dialog anzeigen](variables/variables-in-pipeline-formular-anzeigen.md)

  1. gitlab - ci/cd - Pipelines strukturieren / Templates 
     * [Includes mit untertemplates](gitlab/include/pipeline-with-includes.md)
     * [Parent/Child Pipeline](/gitlab/parent-child-pipeline.md)
     * [Multiproject Pipeline / Downstream](/gitlab/multiproject-pipeline.md)
     * [Vorgefertigte Templates verwenden](gitlab/include/templates.md)
     * [Arbeiten mit extend und anchor - Dinge wiederverwenden](gitlab/cases/extend-and-anchor.md)
    
  1. gitlab - wann laufen jobs ? 
     * [Job nur händisch über Pipelines starten](gitlab/rules/only-web.md)
     * [Auch weiterlaufen, wenn Job fehlschlägt](gitlab/jobs/allow_failure.md)

  1. gitlab ci/cd - docker
     * [Docker image automatisiert bauen - gitlab registry](/gitlab/09-use-gitlab-registry.md)
     * [Docker image automatisiert bauen - docker hub registry](gitlab/09a-docker-build-use-docker-hub.md)
    
  1. gitlab ci/cd - Documentation 
     * [gitlab ci/cd predefined variables](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)
     * [.gitlab-ci.yml Reference](https://docs.gitlab.com/ee/ci/yaml/)
     * [Referenz: global -> workflow](https://docs.gitlab.com/ee/ci/yaml/#workflow)
     * [Referenz: global -> default](https://docs.gitlab.com/ee/ci/yaml/#default)

  1. gitlab ci/cd - Documentation - Includes
     * [includes](https://docs.gitlab.com/ee/ci/yaml/includes.html)
     * [includes -> rules](https://docs.gitlab.com/ee/ci/yaml/includes.html#use-rules-with-include)
     * [includes -> rules -> variables](https://docs.gitlab.com/ee/ci/yaml/#rulesvariables)
     * [includes -> templates -> override-configuration](https://docs.gitlab.com/ee/ci/yaml/includes.html#override-included-configuration-values)
     * [includes -> defaults](https://docs.gitlab.com/ee/ci/yaml/includes.html#use-default-configuration-from-an-included-configuration-file)
    
  1. gitlab ci/cd - Documentation - Services
     * [Documentation services](https://docs.gitlab.com/ee/ci/services/)
    
  1. Kubernetes Monitoring 
     * [Prometheus Monitoring Server (Overview)](prometheus/overview.md)
     * [Prometheus mit helm und grafana aufsetzen](prometheus-grafana/install-with-helm.md)
     * [Prometheus helm-chart, setup ingress](prometheus-grafana/z_01_setup_ingress.md)
     * [Prometheus-gui-walkthrough](prometheus-grafana/z_02_explore_prometheus.md)
     * [Prometheus / Übung blackbox exporter](prometheus-grafana/z_blackbox-exporter.md)
     * [Prometheus / Übung ServiceMonitor für neue App aufsetzen](prometheus-grafana/z_03_exercise_service_monitor.md)

  1. Tipps & Tricks 
     * [Netzwerkverbindung zum Pod testen](/tipps-tricks/verbindung-zu-pod-testen.md)
     * [Debug Container neben Container erstellen](kubernetes-networks/debug-container.md)
     
  1. Weiter lernen 
     * [Lernumgebung](https://killercoda.com/)
     * [Bestimmte Tasks lernen](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/)
     * [Udemy Online Training](https://www.udemy.com/course/certified-kubernetes-security-specialist/)
     * [Kubernetes Videos mit Hands On](https://www.youtube.com/watch?v=16fgzklcF7Y)
        
  1. Documentation
     * [References](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/deployment-v1/#DeploymentSpec)
   

## Backlog (gitlab ci/cd) 

  1. gitlab ci/cd (Praxis I)
     * [Variablen überschreiben/leeren](gitlab/cases/variablen-ueberschreiben-leeren.md)
     * [Rules](/gitlab/05-rules.md)

  1. gitlab ci/cd (Praxis II)
     * [Kommandos auf Zielsystem mit ssh ausführen (auch multiline)](gitlab/jobs/script/ssh-multiline.md)

  1. gitlab ci/cd docker compose
     * [Docker compose local testen](gitlab/docker-compose/01-docker-compose-local-testen.md)
     * [Docker compose über ssh](gitlab/docker-compose/02-docker-compose-ssh.md)
     * [Docker compose classic über scp](gitlab/docker-compose/03-docker-compose-rollout-classic.md)

## Backlog (last day)

  1. Kubernetes Deployment Strategies
     * [Deployment green/blue,canary,rolling update](/deployment-strategies-en.md)
     * [Praxis-Übung A/B Deployment](/kubectl-examples/08-ab-deployment.md)
    
  1. Kubernetes QoS / HealthChecks / Live / Readiness
     * [Quality of Service - evict pods](kubernetes/qos-class.md)
     * [LiveNess/Readiness - Probe / HealthChecks](probes/uebung-liveness.md)
     * [Taints / Toleratioins](kubernetes/taints-tolerations.md)
  
## Backlog II

  1. Kubernetes - Überblick
     * [Allgemeine Einführung in Container (Dev/Ops)](overview-docker.md)
     * [Microservices (Warum ? Wie ?) (Devs/Ops)](microservices.md) 
     * [Wann macht Kubernetes Sinn, wann nicht?](/kubernetes/wann-sinnvoll-wann-nicht.md)
     * [Aufbau Allgemein](/kubernetes/architecture.md)
     * [Aufbau mit helm,OpenShift,Rancher(RKE),microk8s](/kubernetes/aufbau-helm-microk8s-kubernetes.md)
     * [Welches System ? (minikube, micro8ks etc.)](welches-system.md)
     * [Installation - Welche Komponenten from scratch](/kubernetes/installation-components-overview.md)

  1. Kubernetes - microk8s (Installation und Management) 
     * [Installation Ubuntu - snap](microk8s/installation-ubuntu-snap.md)
     * [Remote-Verbindung zu Kubernetes (microk8s) einrichten](microk8s/connect-from-remote.md)
     * [Create a cluster with microk8s](microk8s/cluster.md)
     * [Ingress controller in microk8s aktivieren](microk8s/ingress.md) 
     * [Arbeiten mit der Registry](microk8s/registry.md)
     * [Installation Kuberenetes Dashboard](/microk8s/dashboard.md) 

  1. Kubernetes Praxis API-Objekte 
     * [Connect to external database](/databases/connect-to-external-db.md)
     * [Hintergrund statefulsets](/kubernetes/statefulsets.md)
     * [Example stateful set](/kubectl-examples/10-statefulset.md)

  1. Kubernetes - ENV - Variablen für den Container setzen
     * [ENV - Variablen - Übung](/kubernetes/uebungen-env-variablen.md)

  1. Kubernetes Secrets und Encrypting von z.B. Credentials 
     * [Kubernetes secrets Typen](/kubernetes/secrets/secrets.md) 
     * [Sealed Secrets - bitnami](/kubernetes/secrets/sealed-secrets.md) 

  1. Kubernetes - Arbeiten mit einer lokalen Registry (microk8s) 
     * [microk8s lokale Registry](/microk8s/registry.md)

  1. Kubernetes Praxis Scaling/Rolling Updates/Wartung 
     * Rolling Updates (Devs/Ops) 
     * Scaling von Deployments (Devs/Ops) 
     * [Wartung mit drain / uncordon (Ops)](/kubectl/uncordon-drain.md) 
     * [Ausblick AutoScaling (Ops)](/kubernetes/autoscaling.md) 

  1. Helm (IDE - Support) 
     * [Kubernetes-Plugin Intellij](https://www.jetbrains.com/help/idea/kubernetes.html)
     * [Intellij - Helm Support Through Kubernetes Plugin](https://blog.jetbrains.com/idea/2018/10/intellij-idea-2018-3-helm-support/)
  
  1. Kubernetes Storage 
     * [Praxis. Beispiel (Dev/Ops)](/shared-volumes/nfs-multiple.md)

  1. Kubernetes Netzwerk 
     * [Kubernetes Netzwerke Übersicht](kubernetes-networks/overview.md)
     * [DNS - Resolution - Services](kubernetes-networks/dns-resolution-services.md)
     * [Kubernetes Firewall / Cilium Calico](/kubernetes-network/callico/00-simple-example-multi.md)
     * [Sammlung istio/mesh](sammlung-istio.md)

  1. Kubernetes NetworkPolicy (Firewall)
     * [Kubernetes Network Policy Beispiel](kubernetes-networkpolicy/00-simple-exercises-group.md)    

  1. Kubernetes Autoscaling 
     * [Kubernetes Autoscaling](/kubernetes/autoscaling.md)

  1. Kubernetes Storage 
     * Grundlagen (Dev/Ops)
     * Objekte PersistantVolume / PersistantVolumeClaim (Dev/Ops) 
     * [Praxis. Beispiel (Dev/Ops)](/shared-volumes/nfs-multiple.md) 

  1. Kubernetes Networking 
     * [Überblick](/kubernetes-networks/overview.md) 
     * Pod to Pod
     * Webbasierte Dienste (Ingress) 
     * IP per Pod
     * Inter Pod Communication ClusterDNS 
     * [Beispiel NetworkPolicies](/kubernetes-network/callico/00-simple-example-multi.md)

  1. Kustomize
     * [Beispiel ConfigMap - Generator](/kustomize/01-example-configmap.md)
     * [Beispiel Overlay und Patching](/kustomize/02-overlay-example.md)
     * [Resources](/kustomize/resources.md)

  1. Kubernetes Rechteverwaltung (RBAC) 
     * Warum ? (Ops)
     * [Wie aktivieren?](/microk8s/rbac.md)
     * Rollen und Rollenzuordnung (Ops)
     * Service Accounts (Ops)
     * [Praktische Umsetzung anhand eines Beispiels (Ops)](/kubernetes/rbac-create-user-multi.md)

  1. Kubernetes Backups 
     * [Kubernetes Backup](/backups/cluster-backup-kasten-io.md)

  1. Kubernetes Monitoring 
     * [Debugging von Ingress](/kubernetes/debugging-ingress.md)
     * [Ebenen des Loggings](ebenen-des-loggings.md) 
     * [Working with kubectl logs](/kubectl/logs.md)
     * [Built-In Monitoring tools - kubectl top pods/nodes](microk8s/metrics-server.md)
     * [Protokollieren mit Elasticsearch und Fluentd (Devs/Ops)](microk8s/fluent-kibana-elastic-mit-microk8s.md)
     * [Long Installation step-by-step - Digitalocean](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes)
     * Container Level Monitoring (Devs/Ops)
     * [Setting up metrics-server - microk8s](/microk8s/metrics-server.md) 
  
  1. Kubernetes Security 
     * [Grundlagen und Beispiel (Praktisch)](security/grundlagen-security.md)

  1. Kubernetes GUI 
     * [Rancher](/kubernetes-gui/ranchner.md) 
     * [Kubernetes Dashboard](kubernetes-gui/kubernetes-dashboard.md) 

  1. Kubernetes CI/CD (Optional) 
     * Canary Deployment (Devs/Ops) 
     * Blue Green Deployment (Devs/Ops) 

  1. Tipps & Tricks 
     * [Alias in Linux kubectl get -o wide](/kubectl/alias-o-wide.md)
     * [vim einrückung für yaml-dateien](/vim/vim-yaml.md)
     * [kubectl spickzettel](/kubectl/spickzettel.md)
     * [Alte manifests migrieren](/kubectl/convert-plugin.md)
     * [X-Forward-Header-For setzen in Ingress](/ingress-forward-for-header.md)
  
  1. Übungen 
     * [übung Tag 3](/uebungen/tag3.md) 
     * [übung Tag 4](/uebungen/tag4.md) 
  
  1. Fragen 
     * [Q and A](/q-and-a.md)
     * [Kubernetes und Ansible](/kubernetes-and-ansible.md)
  
  1. Interna von Kubernetes 
     * [OCI,Container,Images Standards](docker-alternatives-kubernetes.md)
    
  1. Kubernetes Administration /Upgrades 
     * [Kubernetes Administration / Upgrades](kubernetes-cluster-update-path.md)
     * [Terminierung von Container vermeiden](avoid-termination-container.md)
     * [Praktische Umsetzung RBAC anhand eines Beispiels (Ops)](/kubernetes/rbac-create-user-multi.md)
   
  1. Andere Systeme / Verschiedenes  
     * [Kubernetes vs. Cloudfoundry](kubernetes-vs-cloudfoundry.md)
     * [Kubernetes Alternativen](kubernetes-alternatives.md)
     * [Hyperscaler vs. Kubernetes on Premise](hyperscaler-vs-kubernetes.md)
     
  1. Lokal Kubernetes verwenden 
     * [Kubernetes in ubuntu installieren z.B. innerhalb virtualbox](/microk8s/installation-ubuntu-snap.md)
     * [minikube](/minikube/installation.md)
     * [rancher for desktop](https://github.com/rancher-sandbox/rancher-desktop/releases/tag/v1.9.1)
     
  1. Microservices 
     * [Microservices vs. Monolith](/microservices/monolith-vs-microservice.md)
     * [Monolith schneiden/aufteilen](/microservices/monolith-schneiden.md)
     * [Strategic Patterns - wid monolith praktisch umbauen](microservices/strategic-patterns.md)
     * [Literatur von Monolith zu Microservices](https://www.amazon.de/Vom-Monolithen-Microservices-bestehende-umzugestalten/dp/3960091400/)
 
  1. Extras 
     * [Install minikube on wsl2](installer/minikube-wsl2.md)
     * [kustomize - gute Struktur für größere Projekte](/kustomize/kustomize-big-projects.md)
     * [kustomize with helm](https://fabianlee.org/2022/04/18/kubernetes-kustomize-with-helm-charts/)

  1. Documentation
     * [Kubernetes mit VisualStudio Code](https://code.visualstudio.com/docs/azure/kubernetes)
     * [Kube Api Ressources - Versionierungsschema](/kubernetes/api-versionierung-lifetime.md)
     * [Kubernetes Labels and Selector](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)
       
  1. Documentation (Use Cases) 
     * [Case Studies Kubernetes](https://kubernetes.io/case-studies/)
     * [Use Cases](https://codilime.com/blog/harnessing-the-power-of-kubernetes-7-use-cases/)

  1. Documentation (Komponenten)
     * [controller manager](https://github.com/kubernetes/kubernetes/tree/release-1.29/cmd/kube-controller-manager/app/options)
