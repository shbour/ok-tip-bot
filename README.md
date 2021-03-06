# Node TIP Bot - OKCash
-----------------------

# Changes
#### Added Bleutrade and Cryptonator.
#### Configured for OKCash http://okcash.co/
###Join us at:
```
irc.freenode.org #okcash
```

# Donations
* BTC `1CNcko4ppGe7P3oJDZW98Cn7Vtpo19korc`
* OK `PGC1FQ8VY8iN3g96L7hxgsrY3fCrr8NyK5`

# Installation
To install the ok-tip-bot simply clone this repo and install dependencies:
```bash
git clone https://github.com/okcashpro/ok-tip-bot
cd ok-tip-bot
npm install
```

# Configuration
To configure, copy the `config/config.sample.yml` file to `config/config.yml`.

## connection
IRC network connection info.
* **host** - hostname of the IRC server
* **port** - port of the IRC server
* **secure** - use secured connection
* **status_command** - NickServ command to get nick's login status, ACC on freenode, STATUS on some other networks

## login
IRC network connection and login info.
* **nickname** - bot's nickname
* **username** - bot's username
* **realname** - bot's realname
* **nickserv_password** - nickserv password to identify with

## channels
List of channels to auto-join to.

## webadmin
Web interface settings.
* **enabled** - enabled web admin
* **port** - port to bound to
* **users** - list of users with access to web interface in `name: password` format

## log
Logging settings.
* **file** - file to log to. Set to `false` to disable logging to file.

## rpc
JSON RPC API connection info.
* **host** - JSON RPC API hostname
* **port** - API port (for okcash)
* **user** - API username
* **pass** - API password (keep that secure)

## coin
Basic coin settings.
* **withdrawal_fee** - fee charged on withdraw to cover up txfee, the rest goes to bot's wallet.
* **min_withdraw** - minimum amount of coins to withdraw
* **min_confirmations** - minimum amount of confirmations needed to tip/withdraw coins
* **min_tip** - minimum amount of coins to tip
* **min_rain** - minimum amount of coins to make rain
* **short_name** - short coin's name (eg. `OK`)
* **full_name** - full coin's name (eg. `OKCash`)

## git
Basic git settings.
* ***enabled*** - `true` or `false`
* ***host*** - `0.0.0.0` or ip of your server
* ***port*** - `3420` or the posrt you want to use, make sure you forward the port
* ***channels*** `#BotGitChannel` channel to post git events

## adding webhooks
In your github repo
* Click Settings > Webhooks & Services > Add Webhook
 * Payload URL your server ip (same as host) or domain eg.
 * Payload URL `http://YourCallBackDomain.com:3420/github/callback`
 * Which events would you like to trigger this webhook? Send me everything
 * Tick Active
 * Add webhook

## urlget
Enable or Disable the bot from responding to URL's typed in the channel.
* Options - 
 * `enabled: true or false`
 *  ***channels*** `#BotChannel` channel to post URL responses

## joke
Enable or Disable the bot from saying a joke with the !joke command.
* Options - 
 * `enabled: true or false`

## random
Enable or Disable the bot from saying a random quote with the !random command.
* Options - 
 * `enabled: true or false`

## allcoin
Enable or Disable the bot from saying your Allcoin coin price with !ticker.
* Options -
 * `enabled: true or false`
 * `coin: OK` Your coins short name
 * `url: https://www.allcoin.com/api2/pair/OK_BTC` Your coins Allcoin api link

## allcoin2
* Options none
  * `url: https://www.allcoin.com/api2/pair/` Dont touch this link 

## bittrex
Enable or Disable the bot from saying your Allcoin coin price with !ticker.
* Options -
 * `enabled: true or false`
 * `coin: OK` Your coins short name
 * `url: https://www.allcoin.com/api2/pair/BTC_OK` Your coins Allcoin api link
  
## bittrex2
* Options none
  * `url: https://bittrex.com/api/v1.1/public/getmarketsummary?market=` Dont touch this link

## cryptsy
Enable or Disable the bot from saying your Cryptsy coin price with !ticker2.
* Options - 
 * `enabled: true or false`
 * `coin: OK` Your coins short name 
 * `url: http://pubapi.cryptsy.com/api.php?method=singlemarketdata&marketid=44` Your coins Cryptsy api link

## btc
Enable or Disable the bot from saying btc price ticker with !btc.
* Options - 
 * `enabled: true or false`

## bleutrade
* Options none
  * `url: https://bleutrade.com/api/v2/public/getmarketsummary?market=OK_BTC` Dont touch this link

## cryptonator
* Options none
  * `url: https://www.cryptonator.com/api/full/ok-btc` Dont touch this link
 
## commands
Here you can restrict some commands to work only on PM/channel.

## messages
Whatever the bot says. Supports expandable variables (eg. `%nick%` for bot's nick). By default all config vars from `rpc` section are available.

# How does it work?
Every nickname has it's own account in your wallet. When tipping or withdrawing, bot checks if user is registered and identified with NickServ. If so, he moves the money from one account to another, or when withdrawing, transfers coins to other wallet.

# How to run it?
Before running the bot, you have to be running your coin daemon with JSON-RPC API enabled. To enable, add this to your coin daemon configuration file (eg. `~/.okcash/okcash.conf`):
```ini
server=1
daemon=1
rpcuser=<your username>
rpcpassword=<your super secret password>
rpcallowip=<your bots ip address or just 127.0.0.1 if hosted on the same machine>
```

# Start The Bot
To run the bot simply use `node bin/tipbot` or `npm start`.

IF you ger this error:
```bash
npm WARN This failure might be due to the use of legacy binary "node"
npm WARN For further explanations, please read
/usr/share/doc/nodejs/README.Debian
```
Run:
```bash
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install python-software-properties python g++ make nodejs
```
If npm isn't available:
```bash

```
## Edits
* To change the ticker for your coin edit -
 * `config/config.yml` line `39 - 50`
 * And add your own Cryptsy, AllCoin and BTC-e coin ticker link.
* Currently only supports Cryptsy, Bleutrade, Cryptonator, AllCoin and BTC-e.
 * See `config/config.yml` line `33 - 50`, `61 -75`, `104 -108`
 * And bin/tipbot.js `267` to `351` to add your own exchange json.

## Commands

| **Command** | **Arguments**     | **Description**                                                                   |
|-------------|-------------------|-----------------------------------------------------------------------------------|
| `balance`   |                   | displays your current wallet balance                                              |
| `address`   |                   | displays address where you can send your funds to the tip bot                     |
| `withdraw`  | `<address>`       | withdraws your whole wallet balance to specified address                          |
| `tip`       | `<nick> <amount>` | sends the specified amount of coins to the specified nickname                     |
| `rain`      | `<amount> [max]`  | sends the specified amount of coins to the channel                                |
| `networkhps`|                   | displays the current network hashpersec with auto speed switch for Kh/s,Mh/s,Gh/s |
| `diff`      |                   | displays the current network difficulty                                           |
| `block`     |                   | displays the current network block                                                |
| `info`      |                   | displays the current network hashpersec/difficulty/block auto switch              |
| `ticker`    | `<coin> or none`  | displays the current Allcoin coin price use an arg, (!ticker DOPE)                |
| `bittrex`   | `<coin> or none`  | displays the current BitTrex coin price use an arg, (!bittrex CANN)               |
| `cryptsy`   |                   | displays the current Crypsty coin price                                           |
| `bleutrade` |                   | displays the current Bleutrade coin price                                         |
|`cryptonator`|                   | displays the current Cryptonator coin price                                       | 
| `btc`       |                   | displays the current BTC-e BTC price                                              |
| `joke`      |                   | displays a random joke                                                            | 
| `random`    |                   | displays a random quote                                                           |
| `help`      |                   | displays configured help message (by default similiar to this one)                |
| `terms`     |                   | displays terms and conditions for using the tip bot                               |
