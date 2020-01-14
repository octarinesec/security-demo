<p align="center">
  <a href="https://octarinesec.com">
    <img src="./img/logo_only.png?raw=true" width="200"/>
  </a>
</p>

# Octarine Security Demo


This repository is set to help people in the community understand Kubernetes security using hands-on, real-life hacking examples. 
Going through the examples listed here, you'll be able to hack into a Kubernetes environments set specially for this purpose to help you think like a hacker for a few minutes.
We hope this experience can help educate you and your team on how critical security is in Kubernetes and can hopfully help raise awareness on ways to prevent security incidents.
Many of the scenarios are based on the work done by [bgeesaman](https://github.com/bgeesaman/k8s-security-demos/blob/master/README.md)  

## How to use it?
This repository currently contains two scenarios, focused mainly on ways to leverage helm tiller privileges to take control over your cluster and gain access to secrets, or workloads, in your cluster.
Each scenario has a full walkthrough with all the assets needed to run it and includes a recorded demo for a quick review.
The demo scenarios:
1. [compromised-container](./compromised-container/README.md)
2. [privilege-escalation](./privilege-escalation/README.md)

## How to contribute?
PR with new scenarios are always welcome! 

