The following will aid in getttnig cAdvisor to use a "slighly" more freindly nameing convention, be make sure you refreence the video to porper plavement of othe inforation provided bellow.

After installing dashbard 14282

Navegate to Home-Dashboards-Cadvisor exporter-Settings-Variables

You should see 

 -Host 
 -Container
 
We will modify both

Under Host, change metic to 
```
container_memory_usage_bytes
```
<img width="680" height="173" alt="Screenshot 2026-02-15 at 2 04 34 PM" src="https://github.com/user-attachments/assets/0d132dbd-07f6-4823-a3db-6a1bf039a450" />

Under Continer, change metic to 
```
container_memory_usage_bytes
```
and Cange lable filters to instance and $host and lable to id

<img width="500" height="172" alt="Screenshot 2026-02-15 at 2 03 39 PM" src="https://github.com/user-attachments/assets/c9f3a376-d59a-4aec-a618-b8a2633e5305" />

# To make each graph read porperly edit the following uner each

# CPU
```
sum(rate(container_cpu_usage_seconds_total{instance=~"$host",id=~"$container"}[5m])) by (container_short_id) *100
```
```
Legend  {{container_short_id}}
```
# Memory	usage
```
sum(container_memory_rss{instance=~"$host",id=~"$container"}) by (container_short_id)
```
```
Legend  {{container_short_id}}
```
# Memory cached
```
sum(container_memory_cache{instance=~"$host",id=~"$container"}) by (container_short_id)
```
```
Legend  {{container_short_id}}
```
# Received Network traffic
```
sum(rate(container_network_receive_bytes_total{instance=~"$host",id=~"$container"}[5m])) by (container_short_id)
```
```
Legend  {{container_short_id}}
```
# Edit Sent Network Traffic:
```
sum(rate(container_network_transmit_bytes_total{instance=~"$host",id=~"$container"}[5m])) by (container_short_id)
````
```
Legend  {{container_short_id}}
```
# Container info
```
(time() - container_start_time_seconds{instance=~"$host",id=~"$container"})/86400
```
```
{{container_short_id}}
```
