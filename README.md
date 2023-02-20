SonarQube Helm Chart
=================

About this Repo
----------------

This is a cloned Git repo of the SonarSource Helm Chart for [SonarQube]:
https://github.com/SonarSource/helm-chart-sonarqube/tree/master/charts/sonarqube

Goal
----------------

The goal of this repo is to have Sonarqube installed on OCP4 (OpenShift Container Platform). As explained in Sonarqube documentation the helm chart is prepared to deploy on plain kubernetes but some changes need to be done in order to deploy on Openshift.

Versions
----------------

OCP4 - Openshift Container Platform 4.12
Sonarqube - SonarQube 9.9 LTS

Prerrequisites
--------------------------

- OCP4 up and running.

Instructions
--------------------------

To install the chart:

git clone https://github.com/anmiralles/sonarqube-ocp4.git
cd ./sonarqube-ocp4/charts/sonarqube
helm repo add sonarqube https://SonarSource.github.io/helm-chart-sonarqube
helm repo update
oc new-project sonarqube
helm upgrade --install -f values.yaml -n sonarqube sonarqube ./

Changes to default helm chart
--------------------------

As showed per git diff:

