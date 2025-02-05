### 2024-07-01

Hello, Internet 🌎 Despite the great journey filled with challenges and learnings, I haven’t found product market fit for this project. Given this, and my current lack of bandwidth and will to continue improving and maintaining it, I’ve arhivie it and move forward. However, I believe this project still holds potential, and I hope someone with the necessary passion and resources might take it forward. 

If you’re interested, feel free to fork the repo and continue its development.

Thank y'all for your support and understanding.

--- 

<p align="center">
  <a href="https://o2pay.co">
    <img src="./.github/static/cover.svg" height="200" alt="cover">
  </a>
</p>

**OxygenPay** is a cloud or self-hosted crypto payment gateway.
Receive crypto including stablecoins with ease. Open new opportunities for your product by accepting cryptocurrency.

<img src="./.github/static/demo.jpg" alt="demo">

## Supported Currencies 🔗

<table>
    <tr>
        <td align="center">
            <img src="./ui-dashboard/src/assets/icons/crypto/eth.svg" height="64" alt="eth">
            <div>Ethereum</div>
        </td>
        <td>
            <img src="./ui-dashboard/src/assets/icons/crypto/matic.svg" height="64" alt="matic">
            <div>Polygon</div>
        </td>
        <td align="center">
            <img src="./ui-dashboard/src/assets/icons/crypto/tron.svg" height="64" alt="tron">
            <div>TRON</div>
        </td>
        <td align="center">
            <img src="./ui-dashboard/src/assets/icons/crypto/bnb.svg" height="64" alt="bnb">
            <div>BNB</div>
        </td>
        <td align="center">
            <img src="./ui-dashboard/src/assets/icons/crypto/usdt.svg" height="64" alt="usdt">
            <div>USDT</div>
        </td>
        <td align="center">
            <img src="./ui-dashboard/src/assets/icons/crypto/usdc.svg" height="64" alt="usdc">
            <div>USDC</div>
        </td>
    </tr>
</table>

## Features ✨

- Self-hosted
- Non-custodial
- Built-in multi-tenancy
- Create payment links for predefined invoices
- Automatic hot wallets management
- Built-in KMS (Key Management Service) for securely storing wallet keys
- Nice and simple merchant dashboard; sleek payment UI
- Easy integration via [API](https://docs.o2pay.co/specs/merchant/v1/) or [webhooks](https://docs.o2pay.co/webhooks)
- No need to setup full-nodes
- Support for testnets
- It's only 1 binary!


## Roadmap 🛣️

- [x] Support for USDC
- [x] Support for Binance Smart Chain (BNB, BUSD)
- [ ] Donations feature
- [ ] Support for [WalletConnect](https://walletconnect.com/)
- [ ] SDKs for (Python, JavaScript, PHP, etc...)
- [ ] Support for all major ETH Layer 2 Chains
- [ ] Support for blockchain notification providers other than Tatum
- [ ] Integration with DEXes for automatic swaps: convert incoming crypto to stablecoins

## License 📑

This software is licensed under [Apache License 2.0](./LICENSE).

<br><br>

## Deployment
Deployment prerequisits. Make sure you have forked, created, or obtained the following:
- A domain name
- A VPS with docker and git installed.
- Tatum.io API keys
- A fork of this repository



Then make sure you setup your `oxygen.env` file. You can do so by copying the example `.env.example` template and fill in the blanks:

```
# Oxygen
WEB_PORT=80
DB_DATA_SOURCE="host=postgres sslmode=disable dbname=oxygen user=oxygen password=oxygen pool_max_conns=32"
SESSION_FS_PATH=/app/sessions

# Random secure string
SESSION_SECRET=<required>

CORS_ALLOW_ORIGINS=https://pay.your-site.com
PROCESSING_WEBHOOK_BASE_PATH=https://pay.your-site.com
PROCESSING_PAYMENT_FRONTEND_BASE_PATH=https://pay.your-site.com
KMS_DB_DATA_SOURCE=/app/kms/kms.db

# Specify initial user here
EMAIL_AUTH_USER_EMAIL=<required>

# Random secure string
EMAIL_AUTH_USER_PASSWORD=<required>

# Providers 
TATUM_API_KEY=<required>
TATUM_TEST_API_KEY=<required>
TRONGRID_API_KEY=<required>

# Random secure string
TATUM_HMAC_SECRET=<required>
```



### Postgre database, configurations and emergency access

First connect the postgre database:
```
 psql -h localhost -p 5432 -U oxygen -d oxygen
```
Password is located in docker config. Upon connection you will see the commandline changes to the following:
```
oxygen=#
```

Here are some useful commands:
- Show all tables: `\dt+`
- Exit: \q`
- show a table and its contents: `SELECT * FROM table_name;`


### Updating Tatum subscriptions & URL Webhook links
If you are changing the following fields in oxygen.env:
```
PROCESSING_WEBHOOK_BASE_PATH=https://pay.your-site.com
TATUM_API_KEY=""
TATUM_TEST_API_KEY=""
```

You need to clear the wallet's table of its tatum subscription reference IDs to trigger a resubscription to tatum.

Be sure to first connect to the database!

```
UPDATE wallets
SET tatum_mainnet_subscription_id = NULL, tatum_testnet_subscription_id = NULL;
```


### Updating source code
Since this project uses docker. Every development/source code changes would mean that the project has to be rebuild to create a fresh image

You can use the preconfigured github actions to do this.
1. Create a new Release on github. This will trigger a build action.
2. Pull the new release github package onto docker using:
```
docker pull ghcr.io/zenovak/oxygen:latest
```
3. Run docker compose again to load the new changes:
```
docker compose up -d
```
