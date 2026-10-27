# Murael Electronics Telegram Bot

Python Telegram bot with SQLite product catalog, bilingual English/Amharic UI, cart, order requests, admin controls, stock management, CSV/XLSX import, and order notifications.

## Important security

- The token previously shared in chat must be revoked in BotFather. Use only the newly generated token in your local `.env`.
- Never share `.env` or commit it to Git.
- `ADMIN_USER_IDS` must contain your personal numeric Telegram user ID(s), not the bot's username or bot ID.
- An order request is not a payment confirmation. Confirm stock, delivery and payment manually.

## Windows setup

Open PowerShell:

```powershell
Set-Location "D:\MuraelElectronicsBot"
& "D:\python.exe" -m venv .venv
& ".\.venv\Scripts\python.exe" -m pip install --upgrade pip
& ".\.venv\Scripts\python.exe" -m pip install -r requirements.txt
Copy-Item .env.example .env
notepad .env
```

If `.venv` already exists, do not recreate it; install dependencies and edit `.env`.

## Configure token and admin ID

In `.env`, set:

```dotenv
TELEGRAM_BOT_TOKEN=YOUR_NEW_TOKEN
BUSINESS_NAME=Murael Electronics
BUSINESS_CONTACT=@Murael21bot
ADMIN_USER_IDS=123456789
DATABASE_PATH=data/murael.db
```

Replace `YOUR_NEW_TOKEN` and `123456789` locally. To discover your numeric Telegram user ID, start the bot and send `/myid` in its private chat. Copy the number into `.env`, restart the bot, and then send `/admin`. Never use the bot's ID.

## Start the bot

```powershell
Set-Location "D:\MuraelElectronicsBot"
& ".\.venv\Scripts\python.exe" bot.py
```

Keep PowerShell open while using polling. `Ctrl+C` stops the bot.

## Customer use

- `/start`: open main menu
- Browse products -> choose a category -> Add
- My cart -> Place order request
- Customer enters name and phone, chooses shop collection or delivery, enters address if delivery, and submits.
- Orders are stored in SQLite and authorized admins are notified.

## Admin use

Set your numeric user ID in `ADMIN_USER_IDS`, restart the bot, and send `/admin`.

- Add product: name (English), category (`Phones`, `Computers`, `Devices`, `Components`, `Repairs`, `Other`), ETB price, stock, English description, and optional Telegram photo.
- Update stock: select Update stock, then send `product_id,quantity`, for example `3,12`.
- Import: select Import CSV/Excel and upload a `.csv` or `.xlsx` file.
- View recent orders in admin panel.
- Update order status: `/status ORDER_ID CONFIRMED` (allowed: `NEW`, `CONFIRMED`, `READY`, `COMPLETED`, `CANCELLED`).

## CSV / Excel columns

Required: `name_en`, `category`, `price`, `stock`.
Optional: `name_am`, `description_en`, `description_am`, `photo_file_id`.
Use `sample_products.csv` as a template. For Excel, first worksheet must have column headers in row 1. Photos imported from a spreadsheet require a valid Telegram `photo_file_id`; easiest is to add photos through the admin menu.

## Current limitations

- Product creation asks for English name/description; the CSV importer supports Amharic fields. Admin edit/deactivate features, payment gateways, and a web dashboard are not included in this initial version.
- SQLite is suitable for a single bot instance. For production multi-instance deployment, use a managed persistent database.
- The app reserves stock when an order is submitted. If an order is cancelled later, stock is not automatically returned; adjust stock manually using the admin stock action.
