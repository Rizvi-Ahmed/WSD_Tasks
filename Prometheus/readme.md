Answers to the questions are given below:

1. Prometheus is monitoring toolkit that allow storage, collection and processing of metrics data.
   * Prometheus collects metrics from various exporters, http endpoints in pull based mechanism.
   * It stores the metrics in a time series database with the time stamps.
   * Prometheus provides PromQL to be able to retrive and aggragate metrics data.
   * Prometheus also provides alerting based on the metrics and  thresholds configured.

2. Sample prometheus alert and rules file


   rules.yaml
```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: kubernetes-alerts
  namespace: monitoring
spec:
  groups:
  - name: kubernetes.rules
    rules:
    - alert: PodCrashLooping
      expr: rate(kube_pod_container_status_restarts_total[15m]) > 0
      for: 5m
      labels:
        severity: warning
      annotations:
        summary: "Pod {{ $labels.pod }} is crash looping"
        description: "Pod {{ $labels.pod }} in namespace {{ $labels.namespace }} is restarting frequently."
    
    - alert: HighMemoryUsage
      expr: (container_memory_usage_bytes / container_spec_memory_limit_bytes) > 0.9
      for: 2m
      labels:
        severity: critical
      annotations:
        summary: "High memory usage detected"
        description: "Container {{ $labels.container }} memory usage is above 90%"

```
This is sample rules files deployed in monitoring namespace considering that prometheus is deployed in the monitoring namespace. It contains alerts for pod crashloop and container high memory usage

To load the rules.yaml file it needs to reference in the main prometheus configuration file

```yaml
rule_files:
- "rules.yml"
```

3. To visualize the usage trend of an application metric that is a counter we can the use the following prometheus query

```yaml
rate(your_counter_metric_name[5m])
```