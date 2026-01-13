# Grafana latest docker image https://hub.docker.com/r/grafana/grafana-enterprise/tags
FROM grafana/grafana-enterprise:12.4.0-20904407122-ubuntu

COPY grafana.ini /etc/grafana/grafana.ini

# Copy dashboards to lib folder
COPY dashboard-jsons/ /var/lib/grafana/dashboards
