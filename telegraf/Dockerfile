FROM telegraf:1.37.1 as telegraf

COPY configs/telegraf.conf /etc/telegraf/telegraf.conf

CMD ["telegraf", "--config", "/etc/telegraf/telegraf.conf"]