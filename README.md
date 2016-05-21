# coreos-nginx
NGINX-powered webserver running on Docker and CoreOS

 - https://github.com/mitchellh/vagrant-aws
 - https://www.vagrantup.com/docs/provisioning/docker.html
 - https://coreos.com/os/docs/latest/cloud-config.html
 - https://coreos.com/os/docs/latest/cloud-config-locations.html
 - https://coreos.com/fleet/docs/latest/launching-containers-fleet.html
 - https://www.alfresco.com/blogs/devops/2015/12/03/docker-security-tools-audit-and-vulnerability-assessment/

 - https://github.com/docker/docker-bench-security
 - https://coreos.com/os/docs/latest/coreos-hardening-guide.html

 - http://www.cyberciti.biz/tips/howto-performance-benchmarks-a-web-server.html

Benchmarked by serving a single file on an AWS t2.micro instance with the following results from Apache Bench:
--------------------------------------------------------------------------------------------------------------
Concurrency Level:      1000
Time taken for tests:   177.211 seconds
Complete requests:      500000
Failed requests:        0
Keep-Alive requests:    500000
Total transferred:      123000000 bytes
HTML transferred:       5500000 bytes
Requests per second:    2821.50 [#/sec] (mean)
Time per request:       354.422 [ms] (mean)
Time per request:       0.354 [ms] (mean, across all concurrent requests)
Transfer rate:          677.82 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   1.5      0      58
Processing:   116  354  53.6    346    1422
Waiting:      116  354  53.6    346    1422
Total:        126  354  53.9    346    1422

Percentage of the requests served within a certain time (ms)
  50%    346
  66%    357
  75%    367
  80%    378
  90%    408
  95%    426
  98%    466
  99%    524
 100%   1422 (longest request)
