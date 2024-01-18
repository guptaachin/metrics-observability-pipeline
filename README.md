<!-- Improved compatibility of back to top link: See: https://github.com/guptaachin/metrics-observability-pipeline/pull/73 -->
<a name="readme-top"></a>
<!--
*** Thanks for checking out the merics-observability-pipeline. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Don't forget to give the project a star!
*** Thanks again! Now go create something AMAZING! :D
-->



<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->
<!-- [![Contributors][contributors-shield]][contributors-url] -->
<!-- [![Forks][forks-shield]][forks-url] -->
<!-- [![Stargazers][stars-shield]][stars-url] -->
<!-- [![Issues][issues-shield]][issues-url] -->
[![MIT License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]



<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/guptaachin/metrics-observability-pipeline">
    <img src="images/logo.jpg" alt="Logo" width="135" height="80">
  </a>

  <h3 align="center">merics-observability-pipeline</h3>

  <p align="center">
    Set up your local Observability - metrics pipeline
    <br />
    <a href="https://github.com/guptaachin/metrics-observability-pipeline/issues">Report Bug</a>
    ·
    <a href="https://github.com/guptaachin/metrics-observability-pipeline/issues">Request Feature</a>
  </p>
</div>


<br />
<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#components-of-metric-pipeline">Components of metric pipeline</a></li>
      </ul>
    </li>
    <li><a href="#open-source-tools-used">Open Source tools used</a></li>
    <li>
      <a href="#getting-started">Getting started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
        <ul>
          <li><a href="#access-grafana">Access Grafana [Best Part]</a></li>
        </ul>
        <li><a href="#install-a-node-exporter-on-your-host-machine-for-exporting-your-machine-system-stats">Install a node exporter</a></li>
      </ul>
    </li>
    <!-- <li><a href="#roadmap">Roadmap</a></li> -->
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#faqs">FAQs</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

<br />

<!-- ABOUT THE PROJECT -->
## About The Project

The growth of Cloud Computing has seen a tremendous upward trajectory in the past 10 years. With services distributed in swarm of virtual machines, it has become necessary to set up a solution to monitor the vitals of these machines and services running on them.  
Therefore Observability into these services is extremely critical to achieve those five nines availability (meaning the service is available for 99.999% of the time, that is unavailable for not more than 5 minutes and 15 seconds a year)  

Observability has three main pillars- metrics, logs and traces.
While logs are more common tool for gauging the behaviour, metrics are as much if not more important. Metrics are time series data, composed of the vitals reported by your service or machine. For example, the `number_of_cpu_cores_used` by a service can be represented as `[(t1, 0.1), (t2, 0.23), (t3, 0.05), (t4, 0.3)]` where `[t1, t2, t3, t4]` are the timestamps when the cpu core usages `[0.1, 0.23, 0.05, 0.3]` were reported.  

<br>

### Components of metric pipeline
This repo will help you set up a simple and complete observability pipeline in your docker environment. The metrics-observability-pipeline has the following capabilities: 

1. Set up a collector to collect prometheus format metrics
2. Set up a collector to collect statsd format metrics
3. Set up a metrics database to store and serve the collected metrics
4. Set up a vizualization interface to be able to plot these metrics in using MetricsQL (Victoria Metrics query language)
5. Set up an alerts rules evaluator.
6. Set up an alerts manager to send notifications and manage the evaluated alert rules.
<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Open Source tools used

Here is a list of all the open source tools we will use.

* [Victoria Metrics][VictoriaMetrics-url]
  * [vmagent][vmagent-url]
  * [vmcluster][vmcluster-url]
  * [vmalert][vmalert-url]
* [Grafana][Grafana-url]
* [Telegraf][Telegraf-url]
* [Prometheus Alert Manager][promalertmanager-url]
* [Docker][docker-url]

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started

Let's try this hands on. To get started on setting up your own pipeline, you will need to set up docker and docker-compose.  

### Prerequisites

- Docker set up - Please follow the official [get-docker][get-docker-url] steps to install docker for your operating system. 
- Docker compose set up - Please follow the official [get-docker-compose][get-docker-compose-url] steps to install docker for your operating system.
- Git client - Please follow this [git-guide] to install git client for your operating system.

### Installation

1. Clone the repo
   ```
   git clone https://github.com/guptaachin/metrics-observability-pipeline.git
   ```
2. Change directoy into `metrics-observability-pipeline`
   ```
   cd metrics-observability-pipeline
   ```
3. Bring up the metrics-observability-pipeline
   Run
   ```
   ./start-mop
   ```
   If that errors out for windows just run the docker compose command directly
   ```
   docker-compose -f ./docker-compose-mop.yaml up -d --force-recreate --build --remove-orphans
   ```
4. It might take a while for the previous command to run as it downloads the standard docker images for the open source tools.
5. Run
   ```
   docker ps | grep mop
   ```
   You will see all the mop container up and running
```
 % docker ps | grep mop
d9cb8acbc384   victoriametrics/vmagent:v1.83.1             "/vmagent-prod --pro…"   41 seconds ago   Up 41 seconds   0.0.0.0:8429->8429/tcp                                                      mop-vmagent
94c9c8d78b43   victoriametrics/vminsert:v1.83.1-cluster    "/vminsert-prod --st…"   42 seconds ago   Up 41 seconds   0.0.0.0:8480->8480/tcp                                                      mop-vminsert
8a94eb7ea359   victoriametrics/vmalert:v1.83.1             "/vmalert-prod --dat…"   46 seconds ago   Up 46 seconds   0.0.0.0:8880->8880/tcp                                                      mop-vmalert
a7409d3288a3   victoriametrics/vmselect:v1.83.1-cluster    "/vmselect-prod --st…"   47 seconds ago   Up 46 seconds   0.0.0.0:8481->8481/tcp                                                      mop-vmselect
36352fd7ba55   victoriametrics/vmstorage:v1.83.1-cluster   "/vmstorage-prod --s…"   48 seconds ago   Up 47 seconds   0.0.0.0:64250->8400/tcp, 0.0.0.0:64251->8401/tcp, 0.0.0.0:64252->8482/tcp   mop-vmstorage
7bdc90ab1fc1   metrics-observability-pipeline_grafana      "/run.sh"                48 seconds ago   Up 47 seconds   0.0.0.0:3000->3000/tcp                                                      mop-grafana
67de46bf371d   prom/alertmanager:v0.24.0                   "/bin/alertmanager -…"   48 seconds ago   Up 47 seconds   0.0.0.0:9093->9093/tcp                                                      mop-alertmanager
31667c83339d   metrics-observability-pipeline_telegraf     "/entrypoint.sh tele…"   48 seconds ago   Up 47 seconds   8092/udp, 8094/tcp, 0.0.0.0:8125->8125/udp                                  mop-telegraf
```
<br>

#### Access Grafana
After the pipeline is up and running 

1. Open a new web-browser window.
2. Launch your locally running Grafana instance `http://localhost:3000/`
3. Punch in Grafana credentials. 
  ```
  username: mopadmin
  password: moppassword
  ```
You can change these login credentials in `grafana.ini` file under `grafana` directory.
4. After logging in try looking at one of the precreated victoria metrics health dashbord by heading over to `http://localhost:3000/d/wNf0q_kZk/victoriametrics?orgId=1&refresh=30s`
<p align="right">(<a href="#readme-top">back to top</a>)</p>

### Install a node exporter on your host machine for exporting your machine system stats
1. Find the right node exporter for yourself from here. [node-exporter-downloads-page]
2. Run wget with the correct link.
3. Unzip. Run `tar xvfz node_exporter-*.*-amd64.tar.gz` in the directory where you wget the node-exporter.
4. Change into the unzip directory. `cd node_exporter-*.*-amd64`
5. Run the node exporter. `./node_exporter`
6. This will expose the machine metrics on 9100. Check these metrics to this link from browser - `http://localhost:9100/metrics`
7. The scrape configs are already included in vmagent.
<br>
> For windows please follow `https://github.com/prometheus-community/windows_exporter`

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## FAQs
### 1. My node exported dashboard does not show up right values. How should I proceed ? 
The node exporter exports different slightly different values depending on your environment. Please import the right dashboard https://github.com/rfmoz/grafana-dashboards/tree/master/prometheus. The current dashboard is https://github.com/rfmoz/grafana-dashboards/blob/master/prometheus/node-exporter-freebsd.json

### 2. How can I deploy this in Kubernetes ?
The most obvious way is to use [Kompose](https://github.com/kubernetes/kompose) for converting the docker compose to a [helm](https://helm.sh/) chart and then use helm commands to do a set up in Kubernetes. Please note I haven't tested it. Setting this up is out of scope of this repo.

### 3. Getting bind address already in use errors. How should I proceed ?
This usually happens when the port an application like Grafana (3000) is already in use by some other application.  
This repo assumes that the standard ports of used open source tools are vacant on the machine you are trying to set up. To solve, either change the ports mapped in docker-compose-mop file or stop the processes hogging those ports.

### 4. What is the username and password for Grafana ?
Please check <a href="#access-grafana">Access Grafana [Best Part]</a>. If you want to change the credentials, you can do so in `/grafana/grafana.ini` file.

### 5. Difference in prometheus queries <PromQL> and queries I write in grafana. How should I proceed ?
This is because while grafana uses Prometheus as the datasource type, it uses Victoria Metrics under the hood. Victoria Metrics under the hood uses [MetricsQL](https://docs.victoriametrics.com/MetricsQL.html) which was created by adding some more features on PromQL.

### 6. Can I used this set up to use for logs or traces ?
You are welcome to fork this repo and contribute. For now, this repo for is only for metrics. 

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->
## License
Distributed under the MIT License. See `LICENSE.txt` for more information.
<br>
Open source libraries like datadog and prometheus-client and tools listed here <a href="#open-source-tools-used">Open Source tools used</a> are trademarks of respective companies. We do not intend to claim credit or blame for the work.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- CONTACT -->
## Contact
Achin Gupta - [Github](https://github.com/guptaachin)

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments
* [Victoria Metrics][VictoriaMetrics-url]
* [Grafana][Grafana-url]
* [Telegraf][Telegraf-url]
* [Prometheus Alert Manager][promalertmanager-url]
* [Docker][docker-url]
* [README Template](https://github.com/othneildrew/Best-README-Template)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/github_username/repo_name.svg?style=for-the-badge
[contributors-url]: https://github.com/guptaachin/metrics-observability-pipeline/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/othneildrew/Best-README-Template.svg?style=for-the-badge
[forks-url]: https://github.com/guptaachin/metrics-observability-pipeline/network/members
[stars-shield]: https://img.shields.io/github/stars/othneildrew/Best-README-Template.svg?style=for-the-badge
[stars-url]: https://github.com/guptaachin/metrics-observability-pipeline/stargazers
[issues-shield]: https://img.shields.io/github/issues/othneildrew/Best-README-Template.svg?style=for-the-badge
[issues-url]: https://github.com/guptaachin/metrics-observability-pipeline/issues
[license-shield]: https://img.shields.io/github/license/othneildrew/Best-README-Template.svg?style=for-the-badge
[license-url]: https://github.com/guptaachin/metrics-observability-pipeline/blob/main/LICENSE
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/achin-gupta-63b0aa128/
[product-screenshot]: images/screenshot.png
[VictoriaMetrics-url]: https://victoriametrics.com/
[vmalert-url]: https://docs.victoriametrics.com/vmalert.html
[vmagent-url]: https://docs.victoriametrics.com/vmagent.html
[Grafana-url]: https://grafana.com/
[Telegraf-url]: https://www.influxdata.com/time-series-platform/telegraf/
[vmcluster-url]: https://docs.victoriametrics.com/Cluster-VictoriaMetrics.html
[promalertmanager-url]: https://prometheus.io/docs/alerting/latest/alertmanager/
[node-exporter-downloads-page]: https://prometheus.io/download/#node_exporter
[docker-url]: https://www.docker.com/
[get-docker-url]: https://docs.docker.com/get-docker/
[get-docker-compose-url]: https://docs.docker.com/compose/install/
[git-guide]: https://github.com/git-guides/install-git
