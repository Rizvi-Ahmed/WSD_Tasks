With this task while trying to deploy the elasticsearch using helm version 3.17, I got the following error which is normally seen when thre is  an compatibility issue between the  chart and the helm binary version. 

```bash
[root@kube-m01 elasticsearch]# helm install elasticsearch . -f envs/qa/customer-abc-elasticsearch/values.yaml --debug
install.go:225: 2025-06-01 00:29:49.698959609 +0600 +06 m=+0.297391860 [debug] Original chart version: ""
install.go:242: 2025-06-01 00:29:49.699040257 +0600 +06 m=+0.297472487 [debug] CHART PATH: /root/elasticsearch/elasticsearch_helm/elasticsearch

Error: INSTALLATION FAILED: parse error at (elasticsearch/templates/_helpers.tpl:20): unexpected <define> in command
helm.go:86: 2025-06-01 00:29:52.435797663 +0600 +06 m=+3.034229893 [debug] parse error at (elasticsearch/templates/_helpers.tpl:20): unexpected <define> in command
INSTALLATION FAILED
main.newInstallCmd.func2
        helm.sh/helm/v3/cmd/helm/install.go:158
github.com/spf13/cobra.(*Command).execute
        github.com/spf13/cobra@v1.8.1/command.go:985
github.com/spf13/cobra.(*Command).ExecuteC
        github.com/spf13/cobra@v1.8.1/command.go:1117
github.com/spf13/cobra.(*Command).Execute
        github.com/spf13/cobra@v1.8.1/command.go:1041
main.main
        helm.sh/helm/v3/cmd/helm/helm.go:85
runtime.main
        runtime/proc.go:272
runtime.goexit
        runtime/asm_amd64.s:

```

Unofortunately, I was unable to install the chart using other helm version but I am outlining the process i would use to deploy elasticsearch and generate the deployment.yaml

```bash
# Within the chart folder containing the template

[root@kube-m01 elasticsearch]# pwd
/root/elasticsearch/elasticsearch_helm/elasticsearch

[root@kube-m01 elasticsearch]# ls
Chart.yaml  envs  templates

[root@kube-m01 elasticsearch]# helm template elasticsearch . -f envs/qa/customer-abc-elasticsearch/values.yaml > deployment.yaml
```

This command will render out the entire yaml manifest that will be deployed by the chart

To intall the chart the command is

```bash
helm install elasticsearch . -f envs/qa/customer-abc-elasticsearch/values.yaml 

```