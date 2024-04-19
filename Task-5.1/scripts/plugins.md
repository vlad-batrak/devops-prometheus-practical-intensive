# Kubectl plugins

## 1. Usage **go**-language plugins by Krew

Install and set up Krew on your machine.

<blockquote>
<details>
<summary><b>Details</b></summary> Krew installation

Krew itself is a kubectl plugin that is installed and updated via Krew (yes, Krew self-hosts).

1. Make sure that git is installed.

2. Run this command to download and install krew:

```bash
(
  set -x; cd "$(mktemp -d)" &&
  OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
  ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" &&
  KREW="krew-${OS}_${ARCH}" &&
  curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
  tar zxvf "${KREW}.tar.gz" &&
  ./"${KREW}" install krew
)
```

3. Add the $HOME/.krew/bin directory to your PATH environment variable. To do this, update your .bashrc or .zshrc file and append the following line and restart your shell.

```bash
export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"
```

4. Run `kubectl krew` to check the installation.

</details>
</blockquote>

Download the plugin list:

```bash
kubectl krew update
```

Discover plugins available on Krew:

```bash
kubectl krew search
```
```
NAME                            DESCRIPTION                                         INSTALLED
access-matrix                   Show an RBAC access matrix for server resources     no
advise-psp                      Suggests PodSecurityPolicies for cluster.           no
auth-proxy                      Authentication proxy to a pod or service            no
[...]
```

Choose a plugin from the list and install it:

```bash
kubectl krew install access-matrix
```

Use the installed plugin:

```bash
kubectl access-matrix
```

Keep your plugins up-to-date:

```bash
kubectl krew upgrade
```

Uninstall a plugin you no longer use:

```bash
kubectl krew uninstall access-matrix
```

## 2. Usage **bash**-language plugins

To use a plugin, make the plugin executable:

```bash
sudo chmod +x ./kubeplugin
```

and place it anywhere in your PATH:

```bash
sudo mv ./kubeplugin /usr/local/bin/kubectl-kubeplugin
```

You may now invoke your plugin as a kubectl command:

```bash
kubectl kubeplugin [flags] <args>
```

All args and flags are passed as-is to the executable:

```bash
kubectl kubeplugin version
```
```
1.0.0
```

All environment variables are also passed as-is to the executable:

```bash
export KUBECONFIG=~/.kube/config
kubectl kubeplugin config
/home/<user>/.kube/config
KUBECONFIG=/etc/kube/config kubectl kubeplugin config
/etc/kube/config
```

Additionally, the first argument that is passed to a plugin will always be the full path to the location where it was invoked ($0 would equal /usr/local/bin/kubeplugin in the example above).


