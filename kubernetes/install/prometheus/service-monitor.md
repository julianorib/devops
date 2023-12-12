## Service Monitor (CRD - Custom Resources Definitions)

https://fabianlee.org/2022/07/07/prometheus-monitoring-a-custom-service-using-servicemonitor-and-prometheusrule/
https://medium.com/daemon-engineering/exposing-metrics-to-prometheus-with-service-monitors-326f38b2daf1
https://medium.com/daemon-engineering/exposing-metrics-to-prometheus-with-service-monitors-326f38b2daf1



### Service
```
apiVersion: v1
kind: Service
metadata:
  name: app-example-service
spec:
  selector:
    app: nginx
  ports:
    - name: http
      port: 80
      targetPort: 80
    - name: http-metrics
      port: 81
      targetPort: 81
```

### ServiceMonitor
```
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: app-example-monitor
spec:
  endpoints:
  - path: /metrics
    port: http-metrics
    scheme: http
    interval: 60s
  selector:
    matchLabels:
      app: app-example-service
```