# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Environment

This is a Python project that monitors Steam Deck availability and sends Discord notifications. The main script is `notifier.py`.

### Dependencies
Install dependencies using:
```bash
pip install -r requirements.txt
```

Required packages:
- `requests` - for Steam API calls
- `discord-webhook` - for Discord notifications

### Running the Application

Run the main script with required arguments:
```bash
python notifier.py --webhook-url "https://discord.com/api/webhooks/YOUR_WEBHOOK"
```

Common command line options:
- `--webhook-url`: Discord webhook URL (required)
- `--country-code`: Country code for Steam API (default: DE)
- `--role-mapping`: JSON file for Discord role pings (optional)
- `--csv-dir`: Directory for CSV logging (optional)

Full example:
```bash
python notifier.py \
  --country-code US \
  --webhook-url "https://discord.com/api/webhooks/YOUR_WEBHOOK" \
  --role-mapping roles.json \
  --csv-dir csv-logs
```

## Architecture

### Core Components

**Main Script (`notifier.py`)**:
- Single-file Python application
- `main()` function orchestrates checking all Steam Deck models
- `superduperscraper()` function handles individual model checking
- Uses Steam's `IPhysicalGoodsService/CheckInventoryAvailableByPackage` API

**Data Persistence**:
- Stores last known availability status in text files named `{package_id}_{country_code}.txt`
- Optional CSV logging to track availability over time with daily files

**Steam Deck Models Monitored**:
- 64GB LCD (Package ID: 903905)
- 256GB LCD (Package ID: 903906) 
- 512GB LCD (Package ID: 903907)
- 512GB OLED (Package ID: 1202542)
- 1TB OLED (Package ID: 1202547)

### Notification Logic
- Only sends Discord notifications when availability status changes
- Supports optional Discord role pinging via JSON configuration
- Prevents duplicate notifications by comparing with stored previous state

### Configuration Files
- `roles.json`: Maps package IDs to Discord role IDs for targeted notifications
- Format: `{"package_id": "discord_role_id"}`