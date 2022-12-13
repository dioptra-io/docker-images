Docker image for https://github.com/YvesVanaubel/TNT.  
Uses the version rebased on scamper-cvs-20211212e at https://github.com/maxmouchet/scamper/tree/scamper-tnt.

```bash
docker compose up -d
docker compose exec tnt sc_tnt -p 9999 -i 128.59.105.24 -o trace.warts
docker compose exec tnt sc_tnt -d 1 trace.warts
```