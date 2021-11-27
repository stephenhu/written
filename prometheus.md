# prometheus

prometheus supports pushing data, but highly recommends pulling data from sources for scalability and isolating failures.  however, if you really need a push mechanism,
there's a push gateway:  https://github.com/prometheus/pushgateway

* https://github.com/prometheus/client_golang
* https://prometheus.io/docs/prometheus/latest/installation/
* https://dzone.com/articles/go-microservices-part-15-monitoring-with-prometheu
* https://www.scalyr.com/blog/prometheus-metrics-by-example/
* https://prometheus.io/docs/instrumenting/exposition_formats/
* https://sysdig.com/blog/prometheus-metrics/
* https://prometheus.io/docs/visualization/grafana/
* https://royportas.com/posts/2020-02-25-prometheus-and-grafana-configuration-for-docker-compose/
* https://blog.pvincent.io/2017/12/prometheus-blog-series-part-2-metric-types/
* https://prometheus.io/docs/practices/naming/#