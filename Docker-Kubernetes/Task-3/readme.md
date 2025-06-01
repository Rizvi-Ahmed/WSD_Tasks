Possible reason for java app restart

* Pod container is using 951Mi which is close to 1000Mi memory request and 1500Mi memory limit. Futher more java heap is configured to 1000M. Combining the java heap and other non heap components using the memory and memory usage of the container base image, the usage may execeeds the memory limit causing kubelet the kill the pod.

* Liveness and readiness probe may be configured wrongly ot too freuently causing kubelet to restart the pod when the health check fais.

To fix the restart possible approach can be

* Increrase memory limit of pod to 2000M and observe.
* Configure liveness and readiness probe to check over longer interval.