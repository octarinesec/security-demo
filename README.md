<p align="center">
  <a href="https://octarinesec.com">
    <img src="./img/logo_only.png?raw=true" width="200"/>
  </a>
</p>

# Octarine Security Demo
Welcome to the Octarine Security Demo. Below you will find two sample scenarios that will allow you to operate like a hacker to better understand how easy it can be to exploit a Kubernetes cluster. 

The Kubernetes environments below have been set up specially for this purpose and are based on the work done by [bgeesaman](https://github.com/bgeesaman/k8s-security-demos/blob/master/README.md) . They take about four minutes each to run. 

The goal of the demo is to raise awareness of the need for improved Kubernetes hardening at inception and at runtime. Our team is continually working to add more scenarios, so keep checking back. 

## How to use it?
This repository currently contains two scenarios, focused mainly on ways to leverage helm tiller privileges to take control over your cluster and gain access to secrets, or workloads, in your cluster. Each scenario has a full walkthrough with all the assets needed to run it and includes a recorded demo for a quick review. The demo scenarios:
1. [compromised-container](./compromised-container)
2. [privilege-escalation](./privilege-escalation)

## How to contribute?
We always welcome PRs with new Kubernetes security scenarios