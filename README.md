SonarQube Helm Chart
=================

About this Repo
----------------

This is a cloned Git repo of the SonarSource Helm Chart for [SonarQube]: https://github.com/SonarSource/helm-chart-sonarqube/tree/master/charts/sonarqube

Goal
----------------

The goal of this repo is to have Sonarqube installed on OCP4 (OpenShift Container Platform). As explained in Sonarqube documentation the helm chart is prepared to deploy on plain kubernetes but some changes need to be done in order to deploy on Openshift.

Versions
----------------

    - OCP4 Openshift Container Platform 4.12
    - Sonarqube 9.9 LTS

Prerrequisites
--------------------------

    - OCP4 up and running.

Instructions
--------------------------

To install the chart:

1. git clone https://github.com/anmiralles/sonarqube-ocp4.git
2. cd ./sonarqube-ocp4/charts/sonarqube
3. helm repo add sonarqube https://SonarSource.github.io/helm-chart-sonarqube
4. helm repo update
5. oc new-project sonarqube
6. helm upgrade --install -f values.yaml -n sonarqube sonarqube ./

Changes to default helm chart
--------------------------

As showed per git diff:

```bash
diff --git a/charts/sonarqube/values.yaml b/charts/sonarqube/values.yaml
index c5ddbc9..42947cd 100644
--- a/charts/sonarqube/values.yaml
+++ b/charts/sonarqube/values.yaml
@@ -20,7 +20,7 @@ deploymentStrategy: {}

 ## Is this deployment for OpenShift? If so, we help with SCCs
 OpenShift:
-  enabled: false
+  enabled: true
   createSCC: true

 edition: "community"
@@ -107,7 +107,7 @@ ingress:
   #     - chart-example.local

 route:
-  enabled: false
+  enabled: true
   host: ""
   # Add tls section to secure traffic. TODO: extend this section with other secure route settings
   # Comment this out if you want plain http route created.
@@ -447,24 +447,24 @@ postgresql:
   securityContext:
     # For standard Kubernetes deployment, set enabled=true
     # If using OpenShift, enabled=false for restricted SCC and enabled=true for anyuid/nonroot SCC
-    enabled: true
+    enabled: false
     # fsGroup specification below are not applied if enabled=false. enabled=false is the required setting for OpenShift "restricted SCC" to work successfully.
     # postgresql dockerfile sets user as 1001
     fsGroup: 1001
   containerSecurityContext:
     # For standard Kubernetes deployment, set enabled=true
     # If using OpenShift, enabled=false for restricted SCC and enabled=true for anyuid/nonroot SCC
-    enabled: true
+    enabled: false
     # runAsUser specification below are not applied if enabled=false. enabled=false is the required setting for OpenShift "restricted SCC" to work successfully.
     # postgresql dockerfile sets user as 1001
     runAsUser: 1001
   volumePermissions:
     # For standard Kubernetes deployment, set enabled=false
     # For OpenShift, set enabled=true and ensure to set volumepermissions.securitycontext.runAsUser below.
-    enabled: false
+    enabled: true
     # if using restricted SCC set runAsUser: "auto" and if running under anyuid/nonroot SCC - runAsUser needs to match runAsUser above
     securityContext:
-      runAsUser: 0
+      runAsUser: "auto"
   shmVolume:
     chmod:
       enabled: false
@@ -491,7 +491,7 @@ tests:

 # For OpenShift set create=true to ensure service account is created.
 serviceAccount:
-  create: false
+  create: true
   # name:
   # automountToken: false # default
   ## Annotations for the Service Account
```