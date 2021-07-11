# Benchmark

## Task 1

The objective of this Task is to measure the performance of a given web server. For my tests I've chosen Nginx which is
a web server used as a reverse proxy, load balancer, mail proxy and HTTP cache.

In order to perform such tests, I have used the command `ab` from Apache Benchmark with the following options:

+ -k 
> Enable the HTTP KeepAlive feature, i.e., perform multiple requests within one HTTP session. Default is no KeepAlive.
+ -n (requests)
> Number of requests to perform for the benchmarking session. The default is to just perform a single request which usually leads to non-representative benchmarking results.
+ -c (concurrency)
> Number of multiple requests to perform at a time. Default is one request at a time.
+ -H ‘Accept-Encoding: gzip,deflate’ (custom-header)
> Append extra headers to the request. The argument is typically in the form of a valid header line, containing a colon-separated field-value pair

So, for example, if I want to test the web server with 1000 requests and a concurrency of 100 I would use the following command:
```
ab -k -n1000 -c100 -H 'Accept-Encoding: gzip,deflate' https://www.nginx.com/
```

### Test 1
#### 1000 requests & 0 concurrents
