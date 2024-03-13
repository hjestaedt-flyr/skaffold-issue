# Information

- Skaffold version: v2.10.1
- Operating system: macOS 14.3 (Build 23D56)
- Installed via: homebrew

# Issue 1

```yaml
# src/foobar/kustomize/overlays/issue1/kustomization.yaml
configMapGenerator:
  - name: foobar-config
    envs:
      - local.env
```

### ✅ Success
This command renders successfully for the profile `issue1`:

```bash
skaffold render -p issue1
```

Result: 
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: foobar
---
apiVersion: v1
data:
  NAME: foobar
kind: ConfigMap
metadata:
  name: foobar-config-hbtb84cg8d
```

### ❌ Failure
If a key-value pair is provided, the command fails:

```bash
skaffold render -p issue1 --set="foo=bar"
```

Error:
```bash
running [kustomize build /var/folders/dy/qslwfy7n0jz3wp1gjxs3m32h0000gp/T/4251885249/Users/foobar/src/test/skaffold-set-issue/src/foobar/kustomize/overlays/issue1]
 - stdout: ""
 - stderr: "Error: loading KV pairs: env source files: [local.env]: evalsymlink failure on '/private/var/folders/dy/qslwfy7n0jz3wp1gjxs3m32h0000gp/T/4251885249/Users/foobar/src/test/skaffold-set-issue/src/foobar/kustomize/overlays/issue1/local.env' : lstat /private/var/folders/dy/qslwfy7n0jz3wp1gjxs3m32h0000gp/T/4251885249/Users/foobar/src/test/skaffold-set-issue/src/foobar/kustomize/overlays/issue1/local.env: no such file or directory\n"
 - cause: exit status 1
```

# Issue 2

```yaml
# src/foobar/kustomize/overlays/issue2/kustomization.yaml
configMapGenerator:
  - name: foobar-config
    files:
      - application.yaml=application-foobar.yaml
```

### ✅ Success
This command renders successfully for the profile `issue2`:

```bash
skaffold render -p issue2
```

Result: 
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: foobar
---
apiVersion: v1
data:
  application.yaml: 'name: foobar'
kind: ConfigMap
metadata:
  name: foobar-config-m896ch6d5m
```

### ❌ Failure
If a key-value pair is provided, the command fails:

```bash
skaffold render -p issue2 --set="foo=bar"
```

Error:
```bash
open /Users/foobar/src/test/skaffold-set-issue/src/foobar/kustomize/overlays/issue2/application.yaml=application-foobar.yaml: no such file or directory
```
