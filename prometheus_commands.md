(* Project Documentation: Setting Up Distributed Prometheus

Introduction-
The goal of this project is to set up a Distributed Prometheus Architecture to monitor various nodes across a network efficiently. 
It involves configuring Prometheus, setting up Node Exporters on different machines, and ensuring connectivity across the network.

The Prometheus Node Exporter exposes a wide variety of hardware- and kernel-related metrics.  *)

(* Project steps-
Configured prometheus.yml to scrape metrics from different nodes.
scrape_configs:
 - job_name: 'node_exporter'
   static_configs:
     - targets: ['localhost:9100'] *)

(* Ensured Node Exporter was running with the correct network bindings.
List processes using a specific port *)
sudo lsof -i :<PORT>
ss -tulnp | grep node_exporter

(* Created necessary directories for Node Exporter. *)
mkdir -p /etc/node_exporter
mv node_exporter /usr/local/bin/
rm -f /usr/local/bin/node_exporter
rm -rf /etc/node_exporter
sudo nano /etc/systemd/system/node_exporter.service

(* Allowed Prometheus to scrape Node Exporter by opening port 9100 in firewall and iptables. *)
sudo ufw allow 9100/tcp

(* Check if Node Exporter is running.
 Start Node Exporter *)
 sudo systemctl start node_exporter

(*  Restart Node Exporter *)
 sudo systemctl restart node_exporter

(*  Enable Node Exporter to start on boot *)
 sudo systemctl enable node_exporter
 
(* Test if Node Exporter is accessible *)
 curl http://localhost:9100/metrics

(* Verifying Prometheus Scraping *)
(* Check Prometheus service status *)
 sudo systemctl status prometheus
 
(*  Restart Prometheus service *)
 sudo systemctl restart prometheus

(*  Enable Prometheus to start on boot *)
 sudo systemctl enable prometheus

(* Used the Prometheus UI (http://localhost:9090) to confirm target status. *)
