# Helm - zsh: no matches found: imagePullSecrets\[0\].name=regcred

Really waste me minutes to resolve this problem, as I'm not familar with zsh. `[]` syntax has its meaning in zsh. So there are two simple ways to step aside.

### Change to bash

Well, switch to bash by just entering \`bash\`. And then run you helm install again.

### noglob

Or you can use `noglob`.

```text
noglob helm install --generate-name ./mychart \
  --set imagePullSecrets[0].name=regcred
```

