```bash
# Probe all IPv4 /24 subnets
docker run -it ghcr.io/dioptra-io/docker-images/flashroute:main --probing_rate 100000 --granularity 24 0.0.0.0/0
```
