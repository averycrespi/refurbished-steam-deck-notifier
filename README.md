[![Latest Release](https://img.shields.io/github/v/release/oblassgit/refurbished-steam-deck-notifier?include_prereleases)](https://github.com/oblassgit/refurbished-steam-deck-notifier/releases)
[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/github/license/oblassgit/refurbished-steam-deck-notifier)](https://github.com/oblassgit/refurbished-steam-deck-notifier/blob/main/LICENSE)  
[![GitHub Stars](https://img.shields.io/github/stars/oblassgit/refurbished-steam-deck-notifier?style=social)](https://github.com/oblassgit/refurbished-steam-deck-notifier/stargazers)
[![Forks](https://img.shields.io/github/forks/oblassgit/refurbished-steam-deck-notifier?style=social)](https://github.com/oblassgit/refurbished-steam-deck-notifier/network/members)
[![Discord](https://img.shields.io/discord/1142517154370043974?label=Discord&logo=discord&style=flat)](https://discord.gg/5gpFTMkvJn)
[![Ko-fi](https://img.shields.io/badge/Buy%20me%20a%20coffee-Ko--fi-FF5E5B?logo=kofi&logoColor=white&style=flat)](https://ko-fi.com/looti)
# Refurbished Steam Deck Notifier

This script checks the availability of refurbished Steam Decks on Steam and sends notifications via Pushover API. It queries Steam's API and compares the current stock status with previously stored values.

## 🚀 Features

* Checks the availability of refurbished Steam Decks for a configurable country
* Sends notifications via **Pushover API** when stock availability changes
* Supports different Steam Deck models (LCD & OLED versions)
* Prevents duplicate notifications by storing the last known stock status
* **Optional CSV logging** for availability statistics
* **Command-line arguments** for easy configuration
* **Environment variable configuration** for secure API credentials
* **Test notification support** to verify configuration

## 📋 Requirements

### Install Dependencies

Ensure you have **Python 3.x** installed. Then, install the required dependencies using:

```bash
pip install -r requirements.txt
```

Or install manually:
```bash
pip install requests pushover-complete python-dotenv
```

## 🛠 Setup & Usage

### Setup and Execution

1. First, set up your Pushover credentials (see Configuration section below)
2. Run the script:

```bash
python notifier.py
```

### Configuration

#### Pushover API Setup

1. Create an account at [Pushover.net](https://pushover.net/)
2. Create a new application at [https://pushover.net/apps/build](https://pushover.net/apps/build) to get your API token
3. Copy `.env.example` to `.env`:

```bash
cp .env.example .env
```

4. Edit `.env` with your credentials:

```
PUSHOVER_API_TOKEN=your_application_token_here
PUSHOVER_USER_KEY=your_user_key_here
```

### Command Line Arguments

* `-h`: Provides list of possible Arguments
* `--country-code`: Country code for Steam API (default: `CA`)
* `--csv-dir`: Directory path for daily CSV log files (optional)
* `--test-notification`: Send a test notification to verify configuration

### Full Example

```bash
python notifier.py \
  --country-code US \
  --csv-dir csv-logs
```

### Test Your Setup

After configuring your `.env` file, test the notification system:

```bash
python notifier.py --test-notification
```

### Country Codes

Find valid country codes [here](https://github.com/RudeySH/SteamCountries/blob/master/json/countries.json)

## 💪 Steam Deck Models Monitored

The script checks availability for these models:

* **64GB LCD** (Package ID: 903905)
* **256GB LCD** (Package ID: 903906)
* **512GB LCD** (Package ID: 903907)
* **512GB OLED** (Package ID: 1202542)
* **1TB OLED** (Package ID: 1202547)

## 🔧 How It Works

1. Loads Pushover API credentials from `.env` file
2. Requests stock status for Steam Deck models via Steam's API
3. Compares new status with the last known state stored in text files
4. Sends a Pushover notification if availability changes
5. Optionally logs the check results to a CSV file

## 📊 CSV Logging

When using `--csv-dir`, the script writes one CSV file for each day to the specified directory, with these fields:

* `unix_timestamp`: Time of check
* `storage_gb`: 64, 256, 512, or 1024
* `display_type`: LCD or OLED
* `package_id`: Steam product identifier
* `available`: `True` or `False`

## ⏲️ Running Periodically

This script **does not run continuously**. Use cron (Linux/macOS) or Task Scheduler (Windows) to automate execution.

### Example (Linux/macOS)

Edit your crontab with:

```bash
crontab -e
```

Add this line to check every 3 minutes:

```bash
*/3 * * * * cd /path/to/project && python notifier.py >> /path/to/logfile.log 2>&1
```

## 📦 Dependencies & Attribution

This project uses the following open-source libraries:
- [**pushover-complete**](https://github.com/sander76/pushover_complete) - A comprehensive Python interface for Pushover API
- [**python-dotenv**](https://github.com/theskumar/python-dotenv) - For loading environment variables from `.env` files
- [**requests**](https://github.com/psf/requests) - For making HTTP requests to Steam's API

It also makes use of Valve's public Steam Store API — specifically the  
[`CheckInventoryAvailableByPackage`](https://api.steampowered.com/IPhysicalGoodsService/CheckInventoryAvailableByPackage/v1?origin=https:%2F%2Fstore.steampowered.com) endpoint ([documentation](https://steamapi.xpaw.me/#IPhysicalGoodsService)). Data and trademarks belong to [Valve Corporation](https://www.valvesoftware.com/), owners of Steam and Steam Deck.

Big thanks to all contributors and maintainers of the open-source packages used in this project.

## ❤️ Support

If this project helps you, consider supporting via [**Ko-fi**](https://ko-fi.com/Y8Y41BZ8SM)

## 🥇 Special Thanks

Huge thanks to [leo-petrucci](https://github.com/leo-petrucci) for helping improve the codebase and guiding proper Steam API usage!
