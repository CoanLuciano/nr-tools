# nr-tools
Essential and Life-Saver Tools for Node Runners.

All these tools are helping me a lot with my Lightning Node [Friendspool⚡🍻](http://amboss.space/c/friendspool)
![image](https://github.com/jvxis/nr-tools/assets/108929149/c11e6d29-72ab-44ef-a9cb-bb8af8c5365a)

## Basic Setup
**How to Setup Telegram Bot:**
1. Create a Telegram Bot:
Open the Telegram app and search for the "BotFather" bot.
Start a chat with BotFather and use the /newbot command to create a new bot.
Follow the instructions to set up your bot and obtain the API token.
2. Get Your Chat ID:
Start a chat with your newly created bot.
Visit the following URL in your web browser, replacing <YOUR_BOT_TOKEN> with the actual token you obtained:
bash
`https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates`
Look for the "chat" object within the response. The "id" field in that object is your chat ID.

**Repository Installation:**
1. Git Clone the Repository: `git clone https://github.com/jvxis/nr-tools.git`

## htlcScan.sh
This script checks for pending stuck htlcs that are near expiration height (< 13 blocks). It collects peers of critical htlc and disconnects / reconnects them to reestablish the htlc. Sometimes htlcs are being resolved before expiration this way and thus costly force closes can be prevented.

**How to Setup:**
1. open the code: `cd nr-tools` and `nano htlcScan.sh`
2. Include your Bot Token and Chat ID to receive telegram messages
3. Replace the line 26 `_CMD_LNCLI="/path_to_umbrel/scripts/app compose lightning exec -T lnd lncli"` with your Umbrel diretory Path
4. Optionally set up the `blocks_til_expiry=13` on line 75 to a higher number
5. Save the Script - CTRL + O
6. Leave the editor - CTRL + X
7. Make the script an executable: `sudo chmod +x htlcScan.sh`
8. Setup CRON to run the script every 30 minutes - `sudo crontab -e`
9. Add the line: `*/30 * * * * /bin/bash /home/<USER>/nr-tools/htlcScan.sh`
10. CTRL + O to save and CTRL + X to leave editor

**Done!**

## check_channelsdb_size.sh
This script checks the LND database size and restarts the lND and other services if it is bigger than 12GB. 

**How to Setup:**
1. open the code: `cd nr-tools` and `nano check_channelsdb_size.sh`
2. Include your Bot Token and Chat ID to receive telegram messages
3. Replace on line 4 /path_to_umbrel with the path for your Umbrel Directory `file_path="/path_to_umbrel/app-data/lightning/data/lnd/data/graph/mainnet/channel.db"`
4. Setup with the size that you usually restart LND on line 7 `threshold_size="12000000000"`
5. Replace lines 41 and 48, where is /path_to_umbrel with path for your Umbrel Directory
6. Save the Script - CTRL + O
7. Leave the editor - CTRL + X
8. Make the script an executable: `sudo chmod +x check_channelsdb_size.sh`
9. Setup CRON to run the script every 1 hour - `sudo crontab -e`
10. Add the line: `0 * * * * /bin/bash /home/<USER>/nr-tools/check_channelsdb_size.sh`
11. CTRL + O to save and CTRL + X to leave editor

**Done!**

## service_on_off.py
This is a telegram bot to start, stop and restart Umbrel services. You can use /on name_of_service to start some services

**How to Setup:**
1. open the code: `cd nr-tools` and `nano service_on_off.py`
2. Include your Bot Token to receive telegram messages
3. Replace /path_to_umbrel with your path to Umbrel directory`SCRIPT_PATH = "/path_to_umbrel/scripts/app"`
4. Save the Script - CTRL + O
5. Leave the editor - CTRL + X
6. Install Dependencies: `pip3 install pyTelegramBotAPI`
7. Run the code: `python3 service_on_off.py`

You can also run it with a screen command to keep it executing in background: `screen -S service-on-off python3 service_on_off.py`

**Usage:**
On your telegram app inside the BOT, you can type:
- /on lightning - to turn on lnd
- /off lightning - to turn off lnd
- /boot lighting - to restart lnd
The same can be done with any Umbrel Services. Like, bitcoin, lightning-terminal etc.

**Done!**

## sats4plus.py
This script sells some info about your node every day, channels, and their capacity, and you get some SATs back as payment for this info

**Pre-reqs**
1. You need to set up an account on https://sparkeer.space
2. Then you need to get the API-KEY
3. Click on Node and then Account and click on the button GENERATE APY KEY
![image](https://github.com/jvxis/nr-tools/assets/108929149/6b320d56-7e6a-41f6-a3ca-d404635fe9fa)
4. Open the code: `cd nr-tools` and `nano sats4plus.py`
5. Replace the line 12 with your API KEY: `API_KEY = "SPARKEER_API_KEY"`
6. On line 55 replace /path_to_umbrel with the path to your Umbrel directory: `["/path_to_umbrel/scripts/app", "compose", "lightning", "exec", "lnd", "lncli", "querymc"],`
7. Save the Script - CTRL + O
5. Leave the editor - CTRL + X
6. Install Dependencies: `pip3 install requests`
7. Run the code: `python3 sats4plus.py`

Recommended to execute this code as a Linux Service or with screen: `screen -S sats4 python3 sats4plus.py`

**Done!**

## onchain-fee-calc.py
This code calculates how much on fees you will spend to send an on-chain transaction, considering your available UTXOs

**Pre-reqs**
You need Balance of Satoshis (BOS) installed

**Usage:**
1. Just run `python3 onchain-fee-calc.py`
   
![image](https://github.com/jvxis/nr-tools/assets/108929149/cae392b6-94a7-4663-a0d5-d2179f48f226)

**Check out the Telegram Bot Version** `onchain-fee-calc-bot.py` You only need to replace it with your Bot Token.

**Done!**

## utxo-consolidator.py
Generates a BOS Fund command to consolidate your unspent UTXOS.

**Pre-reqs**
You need Balance of Satoshis (BOS) installed

**Usage:**
1. Just run `python3 utxo-consolidator.py`

** This program only generates the command, so you should first check it, copy, paste and then RUN.

**Done!**

## openchannel-pro.py
This code, generates a channel open command considering only your unspent UTXOs needed. It helps to save sats during this process. So you can open your channels like a PRO!

**How to Setup:**
1. open the code: `cd nr-tools` and `openchannel-pro.py`
2. Replace path_to_umbrel with your path to Umbrel directory`path_to_umbrel = "Your_Path_TO_Umbrel"`
3. Save the Script - CTRL + O
4. Leave the editor - CTRL + X

**Usage:**
1. Just run `python3 openchannel-pro.py`

** This program only generates the command, so you should first check it, copy, paste and then RUN.

**Done!**

## get_node_daily_balance.py
This code runs with your crontab every day and saves your node balance, considering Forwards and Rebalances

**Pre-reqs**
You need Balance of Satoshis (BOS) installed

**How to Setup:**
1. open the code: `cd nr-tools` and `get_node_daily_balance.py`
2. Replace `NODE_NAME = "Your-node-name"` with your Node Alias
3. Replace `FULL_PATH_BOS = "/home/<user>/.npm-global/lib/node_modules/balanceofsatoshis/"`This is very important to set up right to run with Crontab
4. Save the Script - CTRL + O
5. Leave the editor - CTRL + X
6. open crontab with the command `crontab -e`
7. Add a line: `0 0 * * * /usr/bin/python3 /home/<user>/get_node_daily_balance.py >> /home/<user>/node-balance.log 2>&1` Please check if it is the right path in your system.
8. Save the Script - CTRL + O
9. Leave the editor - CTRL + X

** This code will save your node daily balance in the file `/home/<user>/node-balance.log`

** If you want to run adhoc for a specific date and month please run the code [get_node_balance.py](https://github.com/jvxis/nr-tools/blob/main/get_node_balance.py)

**Done!**
