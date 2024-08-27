Grafana: with loki, promtail.

Observability: Monitoring, alerting, and logs.
Loki is a logs database, Grafana is a dashboard which shows logs in graf format,  and promotel capture the logs from all other soruces.
Promethues helps capturing metrics.


 

First created  linux server and login to it as super user sudo du
Install Grafana on Debian or Ubuntu

Install below dependencies for Grafana, so promotail can capture logs.
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
sudo wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key



Stable release

echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

then install below
sudo apt-get update
sudo apt-get install grafana
sudo systemctl status grafana-server

Now because you are installing services in docker so you need to install docker.
apt-get install docker.io
docker ps
it will show error now. 
So you need to add your user into docker first with below cmd

Sudo usermod -aG docker $USER
Sudo reboot
docker  ps
mkdir grafana_config (I am making this directory to keep loki and promtail config files.)
cd grafana_config
now get loki config file in this folder by downloading below cmd.

Download Loki Config

wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/cmd/loki/loki-local-config.yaml -O loki-config.yaml
Run Loki Docker container

docker run -d --name loki -v $(pwd):/mnt/config -p 3100:3100 grafana/loki:2.8.0 --config.file=/mnt/config/loki-config.yaml



Download Promtail Config

wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/clients/cmd/promtail/promtail-docker-config.yaml -O promtail-config.yaml
Run Promtail Docker container
docker run -d --name promtail -v $(pwd):/mnt/config -v /var/log:/var/log --link loki grafana/promtail:2.8.0 --config.file=/mnt/config/promtail-config.yaml

now you need to read and understand what is inside loki and promtail.

vim loki-config.yaml

don’t forget to open ports for loki and other like 3100 for loki.

After run check if it is running or if it is ready with below cmd:
Ip of your instance:3100/metrics

Now go to Grafana dashboard and add in to datasource your loki ipaddress to fetch data to Grafana. 
Check when you put ipaddress of your ec2 instance is it the same ip in the loki config vim file or else you need to put localhosts:3100

 

Now you need to choose one of the above or filename /job
Now to see jobs, think about where is it getting jobs from in our case it is prom tail which fetches the jobs for it. 
vim promtail-config.yaml

 

Pwd
…………………………………….
 
 

Docker ps
Docker restart (and your container id)
 

Sudo apt-get nginx
 
