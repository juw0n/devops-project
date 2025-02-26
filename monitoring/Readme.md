Observability is an attribute of a system, rather than something you do. It is a system’s ability to be monitored, tracked, and analyzed. Any application worthy of production should be observable.
Analysing system outputs such as metrics, traces, and logs is how you accomplish this:
1. Metrics: typically comprise data collected over time that offers important information about the performance and/or health of an application. 
2. Traces: to give a comprehensive picture, trace a request as it moves through many services.
3. Logs: offer an audit trail of past errors or occurrences that can be utilised for troubleshooting.

The degree of architectural complexity you are working with will ultimately determine what, how, and how much to observe.

Monitoring is any activity that involves keeping track of, evaluating, and sending out alerts based on predetermined criteria in order to determine the present status of a system. 
Applications must report metrics that can provide a narrative about the actions of the system at any given time in order to measure its condition.

The monitoring applications used for this project are:
1. Prometheus: a metric collection application that queries metric data with its powerful built-in query language. It can set alerts for those metrics as well.
2. Alertmanager: receives alerts from Prometheus and determines their path depending on user-configurable parameters. Usually, the routes are notification.
3. Grafana: offers a user-friendly interface for creating and viewing graphs and dashboards using the data that Prometheus supplies.

#### Installing the Monitoring Stack

cd to the root directory of the project and run
==> kubectl apply -R -f monitoring/
chek that the deployment are up and running:
==> kubectl get all -n monitoring
Access the stacks using
==> minikube IP:Dynamic service port for Prometheus
==> minikube IP:Dynamic service port for Grafana
==> minikube IP:Dynamic service port for Alertmanager

Common metric patterns are
1. Golden Signals: (latency, traffic, errors, and saturation)--> help monitor microservices.
2. RED: (Rate, Error, Duration) --> help monitor microservices.
3. USE: (Utilisation, Saturation, Errors) --> for quickly discovering performance issues based on underlying resources (infrastructure) rather than the microservices that run on them