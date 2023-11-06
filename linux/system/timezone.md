# Timezone in Lnux

Configurar timezone correto no Linux.


```
timedatectl
```

```
               Local time: Mon 2023-11-06 17:54:36 UTC
           Universal time: Mon 2023-11-06 17:54:36 UTC
                 RTC time: Mon 2023-11-06 17:54:36
                Time zone: Time zone: Etc/UTC (UTC, +0000)
System clock synchronized: yes
              NTP service: n/a
          RTC in local TZ: no
```

```
timedatectl list-timezones | grep -i Sao_Paulo
```

```
America/Sao_Paulo
```

```
unlink /etc/localtime
```

```
ln -s /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
```

```
timedatectl
```

```
               Local time: Mon 2023-11-06 14:56:58 -03
           Universal time: Mon 2023-11-06 17:56:58 UTC
                 RTC time: Mon 2023-11-06 17:56:58
                Time zone: America/Sao_Paulo (-03, -0300)
System clock synchronized: yes
              NTP service: n/a
          RTC in local TZ: no
```