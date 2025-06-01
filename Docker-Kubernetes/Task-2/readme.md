## To identify the pod restart reason in namespace named production following commands are used.

```bash
kubectl describe pod {pod-name} -n production

kubectl logs {pod-name} -n  production --previous

kuectl get events -n production
```

The above command will allow me to get events and previous logs to identify the restart reason.