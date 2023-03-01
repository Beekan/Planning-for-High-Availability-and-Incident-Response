# API Service

| Category     | SLI                                                          | SLO                                                          |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Availability | Total number of successful requests / total number of requests | 99%                                                          |
| Latency      | buckets of requests in a*histogram* showing the *90th percentile* over the last 5 mins showing requests below 100 ms | 90% of requests below 100ms                                  |
| Error Budget | 1- (1 - successful requests/total requests/(1 - availability)) This gives of the remaining error budget but to get the error budget itself = 1- availability | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget |
| Throughput   | total number of*successful* requests over 5 minutes          | 5 RPS indicates the application is functioning               |
