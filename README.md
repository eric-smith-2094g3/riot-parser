# riot_parser

Parses weather station telemetry dumped from RIOT OS nodes over serial or saved terminal logs. Writes parsed records into JSON or SQLite.

I wrote this because the raw text output from the sensor nodes on my balcony and roof mixes RIOT shell boot spam, hex frames, and ASCII sensor readings.

## Setup

Needs Python 3.10+ on Windows.

```powershell
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

## Running

Parse an existing log dump to JSON:

```powershell
python -m riot_parser --input logs\balcony_dump.txt --output weather.json
```

Stream live from COM port directly into SQLite:

```powershell
python -m riot_parser --port COM3 --baud 115200 --sqlite weather.db
```

Filter by node ID if you have multiple stations repeating over the same link:

```powershell
python -m riot_parser --input logs\repeater.log --node-id 0x2a --sqlite weather.db
```

Binary frames prefixed with `PKT:` get unpacked using the frame layout from our RIOT firmware (`pkt_type`, `seq`, `temp_centi`, `humidity_centi`, `pressure_pa`, `batt_mv`, `crc16`). Frames failing CRC are dropped with a warning to stderr.

If you don't pass `--output` or `--sqlite`, it prints JSON lines straight to stdout.

<!-- last-sync: 2026-09-10 -->
