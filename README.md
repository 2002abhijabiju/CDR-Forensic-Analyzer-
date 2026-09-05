# CDR Forensics Toolkit

A digital forensics tool for analyzing Call Detail Records (CDR), featuring multi-source 
cell tower geolocation and an interactive web-based visualization interface.

## Features

- **CDR Parsing** — ingest and structure call detail records (date, time, number, duration)
- **Cell Tower Geolocation** — resolves tower coordinates from MCC/MNC/LAC/CellID using a 
  waterfall fallback across multiple sources:
  1. Unwired Labs (primary)
  2. BeaconDB (free fallback)
  3. OpenCellID (secondary fallback)
  4. IP-based geolocation (last resort)
- **Weighted Centroid Estimation** — combines multiple tower signals (weighted by signal 
  strength in dBm) to refine location accuracy when more than one tower is available
- **SQLite Storage** — persists parsed CDR and location data (`cdr_forensics.db`)
- **Web Interface** — interactive `index.html` frontend for visualizing call patterns and locations

## Project Structure

## Requirements

- Python 3.12+
- `requests`

Install dependencies:
```bash
pip install requests
```

## Setup

1. Clone the repository:
```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   cd <your-repo>
```

2. Copy the config template and add your own API keys:
```bash
   cp config.json.example config.json
```
```json
   {
     "unwired_api_token": "YOUR_UNWIRED_LABS_TOKEN",
     "opencellid_api_token": "YOUR_OPENCELLID_TOKEN",
     "google_api_key": ""
   }
```
   - Get a free Unwired Labs token: https://unwiredlabs.com
   - Get a free OpenCellID token: https://opencellid.org

3. Run the tool:
```bash
   python main.py
```

4. Open `index.html` in a browser to view the visualization dashboard.

## How Location Resolution Works

For each cell tower reference in a CDR, `location_engine.py` attempts resolution in order:

1. **Unwired Labs** — highest accuracy, requires a paid/free-tier token
2. **BeaconDB** — free, community-sourced cell tower database
3. **OpenCellID** — community database, requires a free token
4. **IP Geolocation** — coarse fallback (~20km accuracy) if no tower match is found

When multiple towers are available for a location estimate, results are combined using a 
**signal-weighted centroid** — towers with stronger signal (higher dBm) are weighted more 
heavily in the final coordinate estimate.

## Disclaimer

This tool is intended for **authorized digital forensics investigations only**. Ensure you 
have proper legal authorization before analyzing any call detail records or subscriber data. 
Handle all CDR data in compliance with applicable data protection and privacy laws.

## Tech Stack

- Python (`requests`, `sqlite3`, `json`)
- HTML/JS — visualization dashboard
- Unwired Labs / BeaconDB / OpenCellID APIs — cell tower geolocation

## License

MIT License (or your preferred license — add a `LICENSE` file).
