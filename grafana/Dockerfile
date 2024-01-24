# Grafana latest docker image https://hub.docker.com/r/grafana/grafana-enterprise/tags
FROM grafana/grafana-enterprise:10.3.1-ubuntu

COPY grafana.ini /etc/grafana/grafana.ini

# Copy dashboards to lib folder
COPY dashboard-jsons/ /var/lib/grafana/dashboards
