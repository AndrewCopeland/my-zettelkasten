# 1599069978 prometheus-metric-types
There are 4 different types of metrics that can be collected using Prometheus.

## Counter
This represents a cumulative metric that increases over time. Like the number of requests to an endpoint.

Do not use Counter to instrument decreasing values, use Gauges instead.
```
# HELP go_memstats_alloc_bytes_total Total number of bytes allocated, even if freed.
# TYPE go_memstats_alloc_bytes_total counter
go_memstats_alloc_bytes_total 3.7156890216e+10
```


## Gauge
Gauges are instantaneous measurements of a value. Values can be arbitrry.

Geages represent a random value than can increase or decrease.
```
# HELP go_goroutines Number of goroutines that currently exist.
# TYPE go_goroutines gauge
go_goroutines 73
```


## Histogram
Samples observations (like request duration or response sizes). A historgam exposes multitple time series during a scrape:
```
# HELP http_request_duration_seconds request duration histogram
# TYPE http_request_duration_seconds histogram
http_request_duration_seconds_bucket{le="0.5"} 0
http_request_duration_seconds_bucket{le="1"} 1
http_request_duration_seconds_bucket{le="2"} 2
http_request_duration_seconds_bucket{le="3"} 3
http_request_duration_seconds_bucket{le="5"} 3
http_request_duration_seconds_bucket{le="+Inf"} 3
http_request_duration_seconds_sum 6
http_request_duration_seconds_count 3
```


## Summary
Similiar to a histogram while also providing a total conount of observations and a sum of all observed values.
It calcualte configurable [quantiles](1599070567-word-quantiles.md) over a sliding time window.

```
# HELP go_gc_duration_seconds A summary of the GC invocation durations.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 3.291e-05
go_gc_duration_seconds{quantile="0.25"} 4.3849e-05
go_gc_duration_seconds{quantile="0.5"} 6.2452e-05
go_gc_duration_seconds{quantile="0.75"} 9.8154e-05
go_gc_duration_seconds{quantile="1"} 0.011689149
go_gc_duration_seconds_sum 3.451780079
go_gc_duration_seconds_count 13118
```


## Source
Most of the notes above came from [here](https://sysdig.com/blog/prometheus-metrics/)

## Links
