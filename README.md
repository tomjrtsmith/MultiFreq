# MultiFreq - The FreqTrade Docker Manager 

* Create a new Freqtrade instance with WebUI in one command line 🚀
* Run multiple instances with the individual config files
* Support HyperOpt and FreqAI functionality
* More to come...

## REQUIREMENTS

* [Docker](https://www.docker.com/)
* 👍 Recommeded: [Vultr Cloud Hosting](https://www.vultr.com/?ref=9282993)| Affordable & Top Performance

## INSTALL
```
git clone https://github.com/nomad5am/MultiFreq.git
cd MultiFreq
./mf
```

## CONFIG

* Create your config in these files `./configs/private`
* Add your Strategies into `./strategies`

## CLI

Run `./mf` in the MultiFreq directory

```
New Instance            ./mf i <instance> create
Start Instance          ./mf i <instance> trade
Stop Instance           ./mf i <instance> stop
List Pairs              ./mf i <instance> pairs <quote>
Create Pairs List       ./mf i <instance> configs-pairs <quote> 
Download Candle Data    ./mf i <instance> data <number-days> <candles> (Format: 5m_15m_1h_1d)
Reset Instance          ./mf i <instance> reset
Remove Instance         ./mf i <instance> remove 
Freqtrade Logs          ./mf i <instance> logs

Run Backtest            ./mf i <instance> backtesting <from-date> (Format: 20211102)
Run HyperOpt & FreqAI   ./mf i <instance> hyperopt <loss_file> <spaces> (Format: buy_sell_roi) <freqai> <from-date> 

Start Web UI            ./mf ui start 
Stop Web UI             ./mf ui stop
```


## CREATE INSTANCE

Suppose you want to create a new instance named `scalper`
```
./mf i scalper create
```
Now set the strategy name and WebUI settings in `./instances/scalper.sh` 

Once that's done update your pairlists and download the latest candle data
```
./mf i scalper configs-pairs USDT
./mf i scalper data 30
```

## RUN HYPEROPT

First, download 30 days of candle data for 5m, 15m, 1h, 1d timeframes
Then launch hyperopt... (See command reference above)
```
./mf i scalper data 30 5m_15m_1h_4h 
./mf i scalper hyperopt SharpeHyperOptLoss buy_sell_trailing_stoploss 5000 
```

## START TRADING 
Launch as many Freqtrade instances with Web UI as your machine can handle
```
./mf i scalper trade
```

## CREDITS

🙏 Thank you [Ph3nol](https://github.com/Ph3nol/FT-Trading-Bot) for starting this project. 
You provided an incredible boilerplate for more things to come!

## (Re)Build Reference Docker images
```
docker pull freqtradeorg/freqtrade:stable
docker buildx build --no-cache --push --platform linux/amd64 \
    --file .docker/freqtrade/Dockerfile \
    --tag ph3nol/freqtrade:latest .

# Update .docker/freqtrade-ui/Dockerfile UI archive version before building Docker image
docker buildx build --no-cache --push --platform linux/amd64 \
    --file .docker/freqtrade-ui/Dockerfile \
    --tag ph3nol/freqtrade-ui:latest .
```

## DISCLAIMER

Do not risk money which you are afraid to lose. 
USE THIS APPLICATION AT YOUR OWN RISK. THE AUTHORS AND ALL AFFILIATES ASSUME NO RESPONSIBILITY ABOUT YOUR TRADING RESULTS.
