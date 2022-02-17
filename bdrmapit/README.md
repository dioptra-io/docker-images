# bdrmapit

bdrmapit [[1]](#references) requires multiple packages and tools to work.
This is an attempt to package everything together, and [fix](#changes) some issues.

## Installation

### Docker 🐳

To get a shell in the current directory with all the tools installed:
```bash
docker run -v $(pwd):/data -w /data -it ghcr.io/dioptra-io/docker-images/bdrmapit:main
```

### Poetry 🐍

You need Python >= 3.7.1 (Pandas requires Python >= 3.7.1).

If you have the right Python version, you can run bdrmapit directly with poetry:
```bash
git clone --recursive https://github.com/dioptra-io/docker-images.git && cd docker-images/bdrmapit/
poetry install
poetry run ip2as ...
poetry run retrieve_external ...
poetry run bdrmapit ...
```

## Using bdrmapit

This is an usage example, refer to the respective tools documentation for more information:
- https://alexmarder.github.io/bdrmapit/
- https://alexmarder.github.io/ip2as/
- https://alexmarder.github.io/retrieve_external/

### Fetching external data

Replace 20210401 with the desired date.

```bash
retrieve_external -b 20210401 -e 20210401 -d ribs bgp
retrieve_external -b 20210401 -e 20210401 -d rirs rir
retrieve_external -b 20210401 -e 20210401 -d rels rels
retrieve_external -b 20210401 -e 20210401 -d ixps peeringdb
# Requires username and password...
# retrieve_external -b 20210401 -e 20210401 -d team caida-team
# retrieve_external -b 20210401 -e 20210401 -d team caida-prefix
```

### Running ip2as

ip2as requires prefix2as files from CAIDA:
```bash
wget https://publicdata.caida.org/datasets/routing/routeviews-prefix2as/2021/04/routeviews-rv2-20210401-1200.pfx2as.gz
wget https://publicdata.caida.org/datasets/as-organizations/20210401.as-org2info.jsonl.gz
```

```bash
find rirs -type f > rir.files
rir2as -f rir.files -r rels/20210401.as-rel.txt.bz2 -c rels/20210401.ppdc-ases.txt.bz2 -o rir.prefixes
ip2as -p routeviews-rv2-20210401-1200.pfx2as.gz -r rir.prefixes -R rels/20210401.as-rel.txt.bz2 -c rels/20210401.ppdc-ases.txt.bz2 -a 20210401.as-org2info.jsonl.gz -P ixps/peeringdb_2_dump_2021_04_01.json -o ip2as.prefixes
```

### Running bdrmapit

Example RIPE Atlas traceroute:
```bash
wget "https://atlas.ripe.net/api/v2/measurements/29792007/results/?start=1619049600&stop=1619135999&format=txt&filename=RIPE-Atlas-measurement-29792007.json" -O 29792007.jsonl
```

Tips: it seems faster to split a large traceroute in multiple smaller files.

```bash
# Configure bdrmapit (see below)...
bdrmapit -o annotations.db -c config.json
```

#### Configuration

```json
{
    "$schema": "schema.json",
    "ip2as": "ip2as.prefixes",
    "as2org": {
        "as2org": "20210401.as-org2info.jsonl.gz"
    },
    "as-rels": {
        "rels": "rels/20210401.as-rel.txt.bz2",
        "cone": "rels/20210401.ppdc-ases.txt.bz2"
    },
    "atlas": {
        "files-list": ["29792007.jsonl"]
    },
    "processes": 8
}
```

## Changes

bdrmapit has been modified as follows:
- run `cython **/*.pyx` and remove `.c` in `.gitignore`.
- replace `pyatlas` with `atlas` in `from traceutils.scamper.pyatlas import ...` in `scripts/traceparser.py`.

traceutils has been modified as follows:
- run `cython **/*.pyx` and remove `.c` in `.gitignore`.
- comment `'traceutils.progress.bar': ...` in setup.py.

## References

[1] Marder, Alexander, et al. ["Pushing the boundaries with bdrmapit: Mapping router ownership at Internet scale."](https://dl.acm.org/doi/pdf/10.1145/3278532.3278538) _Proceedings of the Internet Measurement Conference 2018_. 2018.
