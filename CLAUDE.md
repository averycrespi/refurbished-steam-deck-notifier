# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Environment

This is a Python project that monitors Steam Deck availability and sends Pushover notifications. The main script is `notifier.py`.

### Dependencies
Install dependencies using:
```bash
pip install -r requirements.txt
```

Required packages:
- `requests` - for Steam API calls
- `pushover-complete` - for Pushover notifications
- `python-dotenv` - for loading environment variables

### Environment Setup

Before running the application, set up your Pushover credentials:

1. Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```

2. Edit `.env` with your Pushover credentials:
```
PUSHOVER_API_TOKEN=your_application_token_here
PUSHOVER_USER_KEY=your_user_key_here
```

### Running the Application

Run the main script:
```bash
python notifier.py
```

Common command line options:
- `--country-code`: Country code for Steam API (default: CA)
- `--csv-dir`: Directory for CSV logging (optional)
- `--test-notification`: Send a test notification

Full example:
```bash
python notifier.py \
  --country-code US \
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
- Only sends Pushover notifications when availability status changes
- Prevents duplicate notifications by comparing with stored previous state
- Uses environment variables for secure credential storage

### Configuration Files
- `.env`: Contains Pushover API credentials (not tracked in git)
- `.env.example`: Template for environment variables