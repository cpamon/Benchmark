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

___

### Test 1

| Requests | 1000 |
| ------ | ----------- |
| Concurrency Level   | 1 |
| Time taken for tests | 425.893 seconds |
| Requests per Second    | 2.35 |
| Total transfered    | 24403021 bytes |
| Transfer rate    | 55.96 Kbytes/s |

As expected, with such a low level of concurrency it takes a huge amount of time (425 seconds) to complete the 1000 requests since every request has to be performed once at a time.


![Graph 1](https://github.com/cpamon/Benchmark/blob/main/resultados-1000-0.png)

The graph shows that the latency remains a bit over 200ms until 400 requests. From there it starts to increase almost constantly until the 900th request from which the growth becomes exponential reaching 2000ms at the 1000th request.


___

### Test 2

| Requests | 1000 |
| ------ | ----------- |
| Concurrency Level   | 10 |
| Time taken for tests | 34.451 seconds |
| Requests per Second    | 29.03 |
| Total transfered    | 26666840 bytes |
| Transfer rate    | 755.90 Kbytes/s |

With just a concurrency level of 10 the time taken to complete the 1000 requests is 12,36 times faster than with just 1 concurrency. The transfer rate has also been increased 13.5 times which is a similar value as the time improvement.

![Graph 2](https://github.com/cpamon/Benchmark/blob/main/resultados-1000-10.png)

This time the latency behaviour is a bit different. On the first 100 requests we see a logarithmic growth that rapidly raises the latency over 250ms at the 100th request. From there, there's a constant growth that ends at the 900th request with a latency of 450ms to begin the exponential growth just like in the previous test with the difference that now the 1000th request tops at a bit over 550ms.

Although the latency at the high end values of requests is subtantially lower than in the previous test, I don't see a significant diference in time in the first 800 or 900 requests.

___

### Test 3

| Requests | 1000 |
| ------ | ----------- |
| Concurrency Level   | 100 |
| Time taken for tests | 10.991 seconds |
| Requests per Second    | 90.98 |
| Total transfered    | 26632450 bytes |
| Transfer rate    | 2366.36 Kbytes/s |

In this third test where the concurrency level is 100 we have a great improve in time as well as in the Transfer rate.
The total time to complete the 1000 requests is 3.13 times faster than in Test 2 and the transfer rate is 3.13 times higher than in the previous test.

We can conclude that as we increase the concurrency level we optimise the time taken to perform the whole requests and by doing so we also increase the transfer rate since there are more simultaneous connections.

![Graph 3](https://github.com/cpamon/Benchmark/blob/main/resultados-1000-100.png)

