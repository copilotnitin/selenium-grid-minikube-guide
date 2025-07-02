# 🚀 Selenium Grid on Minikube — Step-by-Step Guide (Windows)

This guide walks through setting up and troubleshooting Selenium Grid 4 using Helm on a local Kubernetes cluster via Minikube and Docker Desktop—fully offline, DNS-agnostic, and cost-free.

---

## 🛠️ 1. Prerequisites

- ✅ Chocolatey: https://chocolatey.org/
- ✅ Docker Desktop (Linux containers enabled)
- ✅ Minikube:
  ```bash
  choco install minikube
  ```
- ✅ kubectl: Download from: https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/ Copy kubectl.exe to a folder in your PATH (e.g. C:\Windows\System32)
- ✅ Install helm
    ```bash 
    choco install kubernetes-helm
    ```
- ✅ git for cloning helm char repo
- ✅ install helm dependencies

## Start minikube
```
- minikube start --driver=docker 
```
```
- kubectl get nodes 
```

## Clone and install Selenium grid helm chart
```
git clone https://github.com/SeleniumHQ/docker-selenium.git
cd docker-selenium/charts/selenium-grid
helm install selenium-grid .
```
### Confirm
```
    kubectl get svc
   ```
## Port forward gitHub
### Check Service name
```
kubectl get svc
```
### Then forward the hub
```
kubectl port-forward svc/selenium-grid 4444:4444 --address=0.0.0.0
```
#### optional - Keep alive powershell
```
while ($true) { iwr http://localhost:4444/status; Start-Sleep 30 }
```
## Connect your framework
### Update test config 
```bash 
  new RemoteWebDriver(new URL("http://localhost:4444/wd/hub"), chromeOptions);
```
### Run with maven
```
mvn clean test -Dbrowser=chrome -DgridUrl=http://localhost:4444/wd/hub
```
## Cleanup
```
helm uninstall selenium-grid
minikube stop
```
## Troubleshooting
- **Helm repo unreachable**	Use git clone to install chart locally
- **SessionNotCreatedException**	Ensure nodes are ready, port-forwarding active, and browser caps match
- **Port-forward dropped**	Use --address=0.0.0.0 and run a heartbeat script
- **Maven testSuiteXmlFiles0 error** Pass -DtestSuite=... or set default in pom.xml
svc not found	Check actual service name via kubectl get svc

