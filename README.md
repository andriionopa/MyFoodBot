# MyFoodBot

MyFoodBot is a Telegram nutrition assistant that analyzes meal photos, estimates calories and macronutrients, tracks daily water intake, and manages paid subscription access.

The bot is built with Python, aiogram, SQLite, and Anthropic Claude vision models. It is designed as a small production Telegram service with environment-based configuration, subscription state, admin operations, and local operational scripts.

## Features

- Meal photo analysis with dish recognition, estimated weight, calories, protein, fat, and carbohydrates
- Daily nutrition summary for the last 24 hours
- Water tracking with quick add actions
- Multi-language user-facing messages
- Free trial usage limits and subscription checks
- Admin commands for users, subscriptions, backups, cleanup, and usage statistics
- SQLite persistence for users, subscriptions, and food analysis history
- Optional payment integrations through Telegram Stars, CryptoBot, LiqPay, provider tokens, and manual wallet details

## Tech Stack

- Python 3.8+
- aiogram 3.x
- Anthropic SDK
- SQLite
- python-dotenv
- Pillow, OpenCV, NumPy

## Repository Layout

```text
.
├── bot.py
├── config.py
├── food_analyzer.py
├── simple_payment.py
├── subscription_db.py
├── subscription_cleanup.py
├── translations.py
├── user_manager.py
├── migrate_subscriptions.py
├── setup_cron.sh
├── start_bot.sh
└── requirements.txt
```

## Requirements

- Python 3.8 or newer
- Telegram bot token from BotFather
- Anthropic API key with access to the configured Claude model
- A server or workstation that can run a long-lived Python process

## Installation

```bash
git clone git@github.com:andriionopa/MyFoodBot.git
cd MyFoodBot
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Configuration

Create a local `.env` file. Do not commit it.

```bash
cp .env.example .env
```

Minimum required values:

```env
TELEGRAM_BOT_TOKEN=replace_with_telegram_bot_token
ANTHROPIC_API_KEY=replace_with_anthropic_api_key
```

Optional values:

```env
CLAUDE_MODEL=claude-3-5-haiku-20241022
CRYPTOBOT_API_TOKEN=replace_with_cryptobot_token
USDT_TRC20_WALLET=replace_with_usdt_trc20_wallet
BTC_WALLET=replace_with_btc_wallet
ETH_WALLET=replace_with_eth_wallet
LIQPAY_PUBLIC_KEY=replace_with_liqpay_public_key
LIQPAY_PRIVATE_KEY=replace_with_liqpay_private_key
PAYMENT_PROVIDER_TOKEN=replace_with_payment_provider_token
```

The application also contains runtime defaults in `config.py` for database location, trial limits, subscription duration, and payment feature flags.

## Running Locally

```bash
source venv/bin/activate
python bot.py
```

The bot starts polling Telegram updates and creates local SQLite data files when needed.

## User Commands

- `/start` opens the main menu
- `/help` shows usage guidance
- `/about` shows project information
- `/language` changes interface language
- `/status` shows subscription status
- `/payment` opens payment instructions
- `/stats` shows daily nutrition statistics

Users can also send a food photo directly to receive a structured nutrition estimate.

## Admin Operations

Admin commands are restricted by Telegram user ID in `bot.py`.

- `/admin_help`
- `/admin_users`
- `/admin_user <user_id>`
- `/admin_user_stats <user_id>`
- `/admin_stats`
- `/admin_subscribe <user_id> <months>`
- `/admin_extend <user_id> <days>`
- `/admin_revoke <user_id>`
- `/admin_reset_trials <user_id>`
- `/admin_add_trials <user_id> <count>`
- `/admin_backup`
- `/admin_cleanup`
- `/cleanup_stats`
- `/migrate_db`

For production use, move administrator IDs to environment configuration before sharing deployments between environments.

## Data Files

Runtime files such as SQLite databases, logs, backups, and `.env` are ignored by Git. Keep backups encrypted or outside the repository if they contain user data.

## Security Notes

- Never commit `.env`, database files, payment tokens, wallet secrets, or exported backups.
- Rotate credentials immediately if they were ever committed or shared.
- Keep the bot token, Anthropic API key, and payment provider credentials in environment variables.
- Review Telegram and payment provider compliance requirements before enabling paid access.

## Validation

```bash
python3 -m py_compile *.py
gitleaks detect --source . --no-banner
```

## License

MIT. See `LICENSE`.
