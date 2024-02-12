In this repo you have the files you need to deploy and monitor nginx web server.
Starting up:
Inside of the Deployment file called nginx.yaml you will find the following:
1.Deployment
2.Pod configuration of nginx pod and second pod that is the nginx prometheus exporter
3.Service
4.Configmap
5.horizontal scaling configuration

You can simply apply this file after configuring the values (if needed).
After applying you need to have helm installed to install the prometheus operator using the following commands:
add the repo:
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

install prometheus stack, not just prometheus to include grafana and more useful dashboard and exporters for your kubernetes cluster:
helm install [RELEASE_NAME] prometheus-community/kube-prometheus-stack

Full documentation and more add-ons for the prometheus stack can be found in the original repo of the operator - https://github.com/prometheus-community/helm-charts/tree/main

After installing you can edit the grafana service to be nodeport to access the dashboard (not recommended) or create ingress that leads to grafana service clusterIP to also assign dedicated URL for the grafana for easy access (recommended)

In order to have the metrics scraped from nginx exporter and into the prometheus you need to apply the service-monitor.yaml configuration to add the nginx exporter endpoints to prometheus scraping list.

In this repo you will also find file called dashboard.json, after adding the service monitor, log into the grafana and choose the Add Dashboard option, choose the upload json file, and upload the dashboard.json, this will give you visibility and graphs of the metrics you scrap from nginx exporter.


Notes:
If you already have prometheus installed on your cluster, skip the prometheus installation part and just apply the service monitor after the nginx installation

I tried to add a non operator option but i did not have enough time, hence the files of the prometheus.yaml, prometheus-service.yaml etc .. So the only files you need from this repo are:
nginx.yaml
service-monitor.yaml
dashboard.json
