# Wymagania dla systemu Windows

## Git

W trakcie warsztatów wymagane będzie pobranie zawartości repozytorium Git. Należy zainstalować klienta Git dla systemu Windows z tego [linku](https://git-scm.com/download/win)

## Kubernetes

Lokalny klaster Kubernetes można zainstalować na potrzeby warsztatów, przy użyciu Minikube. Wymagane jest do tego wsparcie wirtualizacji - należy włączyć w BIOSie.

### Minikube

Przed zainstalowaniem Minikube należy zainstalować [VirtualBox](https://www.virtualbox.org/wiki/Downloads). Następnie należy zainstalować [narzędzie `kubectl`](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-windows) oraz [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/).

### Weryfikacja instalacji lokalnego klastra Kubernetes

Aby uruchomić lokalny klaster Kubernetes należy w wierszu poleceń wykonać polecenie `minikube start --vm-driver virtualbox` jak poniżej.

```console
PS C:\WINDOWS\system32> minikube start --vm-driver virtualbox
* minikube v1.5.2 on Microsoft Windows 10 Pro 10.0.18362 Build 18362
* Creating virtualbox VM (CPUs=2, Memory=2000MB, Disk=20000MB) ...
* Preparing Kubernetes v1.16.2 on Docker '18.09.9' ...
* Pulling images ...
* Launching Kubernetes ...
* Waiting for: apiserver
* Done! kubectl is now configured to use "minikube"
```

Następnie trzeba uruchomić komendę `kubectl version`. Oto przykładowy wynik uruchomienia wspomnianej komendy na poprawnie zainstalowanym klastrze:

```console
PS C:\WINDOWS\system32> kubectl version
Client Version: version.Info{Major:"1", Minor:"16", GitVersion:"v1.16.2", GitCommit:"c97fe5036ef3df2967d086711e6c0c405941e14b", GitTreeState:"clean", BuildDate:"2019-10-15T19:18:23Z", GoVersion:"go1.12.10", Compiler:"gc", Platform:"windows/amd64"}
Server Version: version.Info{Major:"1", Minor:"16", GitVersion:"v1.16.2", GitCommit:"c97fe5036ef3df2967d086711e6c0c405941e14b", GitTreeState:"clean", BuildDate:"2019-10-15T19:09:08Z", GoVersion:"go1.12.10", Compiler:"gc", Platform:"linux/amd64"}
```

Wynik uruchomienia polecenia może być różny w zależności od daty jej wykonania - wystarczy upewnić się, że polecenie poprawnie zwróciło wersje klienta oraz serwera. Jeżeli weryfikacja przebiegła poprawnie można zakończyć działanie lokalnego klastra poleceniem `minikube stop`.

```console
PS C:\WINDOWS\system32> minikube stop
* Stopping "minikube" in virtualbox ...
* "minikube" stopped.
```

## IDE

Sugerowanym edytorem podczas warsztatów jest [Visual Studio Code](https://code.visualstudio.com/) z aktywowanym rozszerzeniem [Kubernetes](https://code.visualstudio.com/docs/azure/kubernetes#_install-the-kubernetes-extension).
