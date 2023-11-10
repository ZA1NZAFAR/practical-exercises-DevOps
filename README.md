# README #

This repository concerns the practical work on the "DevOps and Continuous Deployment" course taught at EFREI in Master 2 (ST2DCE), for a total of 22 hours, representing 12 hours of practical work and 10 hours of project.

Workshops will be organized in 3 sessions of 4 hours. The main objective is to learn how to build, deploy, scale and monitor an application using Docker / Kubernetes and the Prometheus/Loki/Grafana stack for observability.

## Requirements ##

To achieve this goal, you need to have the following tools already installed and ready to use in your working environment:

* Virtulbox with a working Linux distribution: e.g. Fedora, Debian/Ubuntu with root access
On the deployed virtual machine, you must have the following tools installed
  * Docker 24.0.7 or higher
  * Docker compose v2.23.0 or higher
  * Jenkins version jenkins/jenkins:lts-jdk17 with suggested plugins selected by default running on the VM.
  * Git version 2.41.0 or higher

For all the duration of the workshop and the project, it is strongly recommended to keep the working environment functional.

Below is a summary of the content of each workshops.  The TP-1, TP-2 and TP-3 folders contain detailed descriptions and instructions for the work to be carried out.

## TP-1 - Biuld and Deploy pipeline ##

* Build pipeline : build, package and deploy an application using docker
  * Level 1 : using a Dockerfile wihout registry
  * Level 2 : using a Dockerfile with registry
* Deployment orchestration : docker-compose

## TP-2 - Deployment orchestation using Kubernetes ##

* Manage, scale, and maintain containerized applications
  * Deployment in different environments
  * Scalability, deployment strategies ...

## TP-3 - Monistoring and Incidents Management ##

* Monitor the application / platform
  * Install and configure Prometheus / Grafana / AlertManager stack
  * Install node_exporter and view metrics on Grafana
  * Install Loki/promtail (log centralization)c
