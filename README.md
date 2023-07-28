# Grafana
### What is Grafana

Grafana is a popular open-source data visualization and analytics platform that allows you to create custom dashboards and visualizations based on a variety of data sources. Grafana is often used for monitoring and analyzing metrics and logs in real-time, making it an ideal tool for monitoring systems and applications, including Kubernetes environments.

Grafana supports a wide range of data sources, including databases, time-series databases, and other data storage systems. It provides a powerful query language that allows you to retrieve and analyze data from these sources, and to create custom dashboards and alerts based on that data.

In addition to its powerful data visualization and analysis capabilities, Grafana is also highly extensible. It supports a wide range of plugins and integrations, including integrations with popular monitoring and logging tools like Prometheus, Elasticsearch, and InfluxDB.
#### **1.Install Grafana on Debian or Ubuntu**
```
sudo  apt-get  install  -y apt-transport-https

sudo  apt-get  install  -y software-properties-common wget

sudo  wget  -q  -O /usr/share/keyrings/grafana.key [https://apt.grafana.com/gpg.key]
https://apt.grafana.com/gpg.key
```
#### **2.Stable Release**
```
echo  "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main"  |  sudo  tee  -a /etc/apt/sources.list.d/grafana.list
```

#####  A. Update the list of available packages
```
sudo  apt-get update
```
##### B. Install the latest OSS release:
```
sudo  apt-get  install grafana
```

#### Start Grafana
```
sudo systemctl start grafana-server
```
#### Enable Grafana
```
sudo systemctl enable grafana-server
```


### **Install Loki and Promtail using Docker**
#### 1.Download Loki Config  
  ```
wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/cmd/loki/loki-local-config.yaml -O loki-config.yaml
```

1.  `wget`: This is the command itself, which stands for "Web Get." It is a utility for downloading files from the web.
    
2.  `https://raw.githubusercontent.com/grafana/loki/v2.8.0/cmd/loki/loki-local-config.yaml`: This is the URL of the file you want to download. In this case, it is the raw content of the `loki-local-config.yaml` file from the GitHub repository of Grafana's Loki project, specifically version 2.8.0.
    
3.  `-O loki-config.yaml`: This is an option for `wget`, where `-O` specifies the output filename. In this case, it renames the downloaded file to `loki-config.yaml` on your local system.

#### 2.Run Loki Docker container
```
docker run -d --name loki -v  $(pwd):/mnt/config -p  3100:3100 grafana/loki:2.8.0 --config.file=/mnt/config/loki-config.yaml
```
  
The command you provided is a Docker command that runs a container for Grafana Loki, a log aggregation system. Here's a breakdown of the command:

1.  `docker run`: This is the command to run a Docker container.
    
2.  `-d`: This is an option for `docker run` and stands for "detached mode." It runs the container in the background, allowing you to continue using the terminal or command prompt.
    
3.  `--name loki`: This is another option that sets the name of the running container as "loki."
    
4.  `-v $(pwd):/mnt/config`: This is an option to mount a volume inside the container. It links the current working directory (`$(pwd)`) on your local system to the `/mnt/config` directory inside the container. This is used to provide the configuration file (`loki-config.yaml`) from your local system to the container.
    
5.  `-p 3100:3100`: This is an option to publish ports. It maps port 3100 from the container to port 3100 on your local system. This allows you to access Grafana Loki on your local machine through port 3100.
    
6.  `grafana/loki:2.8.0`: This is the Docker image that will be used to create the container. Specifically, it's the Grafana Loki image with version 2.8.0.
    
7.  `--config.file=/mnt/config/loki-config.yaml`: This is a command-line argument for the Loki container. It specifies the location of the configuration file `loki-config.yaml` inside the container. Since we mounted the local directory as a volume, the configuration file from your local system will be available at this path.

#### 3.Download Promtail Config
```
wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/clients/cmd/promtail/promtail-docker-config.yaml -O promtail-config.yaml
```
#### 4.Run Promtail Docker container
```
docker run -d --name promtail -v  $(pwd):/mnt/config -v /var/log:/var/log --link loki grafana/promtail:2.8.0 --config.file=/mnt/config/promtail-config.yaml
```
#### Prometheus vs Loki
<p align="center">
<b>Loki</b><br/></p>
Grafana is one of the leading providers of  software observability Loki is Grafana's log aggregation component. Inspired by Prometheus, it's highly scalable and capable of handling petabytes of log data.


![Alt text](https://grafana.com/docs/loki/latest/fundamentals/architecture/components/loki_architecture_components.svg)


<p align="center">
<b>Prometheus</b><br/> </p>
Prometheus is an open-source monitoring and alerting system that helps you collect and store metrics about your software systems and infrastructure, and analyze that data to gain insights into their health and performance.

![Alt text](https://camo.githubusercontent.com/2cb8b2d19c897fef44f30b6a0c8ba5d0c6ea7ffde700cdd84d3c053ad0406815/68747470733a2f2f70726f6d6574686575732e696f2f6173736574732f6172636869746563747572652e706e67)

### Here I Use Loki
