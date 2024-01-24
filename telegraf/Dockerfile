FROM telegraf:1.29.2 as telegraf

COPY configs/telegraf.conf /etc/telegraf/telegraf.conf

CMD ["telegraf", "--config", "/etc/telegraf/telegraf.conf"]