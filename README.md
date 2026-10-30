# prometheus-grafana-monitoring
linux server monitoring using prometheus grafana 
Prometheus and Grafana Monitoring Project
1. Project overview
This project monitors an Ubuntu Linux machine using Prometheus, Node Exporter, and Grafana.

Prometheus collects and stores time-series metrics.
Node Exporter exposes Linux host metrics such as CPU, memory, disk, and network.
Grafana visualizes Prometheus metrics in dashboards and provides alerting.
2. Architecture
Ubuntu host
   |
   +-- Node Exporter :9100  ----+
   |                            |
   +-- Prometheus :9090 <-------+
            |
            +---- Grafana :3000
3. Components and ports
Component	Default port	Purpose
Prometheus	9090	Scrapes and stores metrics
Node Exporter	9100	Exposes Linux host metrics
Grafana	3000	Dashboards and alerting
4. Installation and service verification
On Ubuntu, the packages used in this lab are:

sudo apt update
sudo apt install prometheus prometheus-node-exporter -y
sudo systemctl enable --now prometheus
sudo systemctl enable --now prometheus-node-exporter
Grafana was installed from the official Grafana APT repository. Start and enable it with:

sudo systemctl enable --now grafana-server
Check services:

sudo systemctl status prometheus
sudo systemctl status prometheus-node-exporter
sudo systemctl status grafana-server
Check endpoints:

curl http://localhost:9090/-/healthy
curl http://localhost:9100/metrics
Open the web interfaces locally:

Prometheus: http://localhost:9090
Prometheus targets: http://localhost:9090/targets
Grafana: http://localhost:3000
5. Prometheus scrape configuration
The example configuration is in prometheus/prometheus.yml.

After editing the live configuration at /etc/prometheus/prometheus.yml, validate and restart:

sudo promtool check config /etc/prometheus/prometheus.yml
sudo systemctl restart prometheus
Verify that the prometheus and node_exporter targets are UP at http://localhost:9090/targets.

The sample file uses localhost because all services in this lab run on the same Ubuntu machine. If Node Exporter runs on another machine, replace localhost:9100 with that machine's reachable IP address and port.

6. PromQL queries
The queries used in this lab and their explanations are documented in queries/promql-queries.md.

Queries include CPU usage, memory usage, total and available memory, root filesystem usage, network receive/transmit rates, uptime, and target health.

7. Grafana configuration and dashboard
Open Grafana at http://localhost:3000.
Add a Prometheus data source.
For this same-machine setup, use http://localhost:9090 as the Prometheus server URL.
Select Save & test and verify that the Prometheus API query succeeds.
Create a dashboard with CPU, memory, disk, network, and uptime panels.
Add an instance dashboard variable if required.
Export the finished dashboard as JSON from Grafana and save it in this repository as:

grafana/prometheus-server-monitoring.json
The grafana/ folder in this starter repository is intentionally empty until you export and add your own dashboard JSON.

8. Alerting
A Grafana-managed test alert was created with:

Query: vector(1)
Condition: query value is above 0
Pending period: 0s
This is a test-only rule. Since vector(1) always returns 1, it is expected to fire continuously. Disable or delete this test rule after capturing the firing-alert evidence.

A real CPU alert can use the CPU usage query and a threshold above 80 with a pending period of 1m. This alert only fires if CPU usage remains above 80% for one minute.
