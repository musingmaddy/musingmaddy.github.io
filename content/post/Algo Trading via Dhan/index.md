---
title: Algo Trading via Dhan API
slug: algo-trading-via-dhan-api
date: 2024-02-25
description: I tried Algorithmic Trading via Dhan API.
image: cover.jpg
weight: 1
tags:
  - Dhan
  - Algo-Trading
  - Options
  - Nifty
  - Python
  - API
categories:
  - Trading
  - Coding
---
# Algorithmic Trading via Dhan API

# Dhan API Algo Trading

## Important Points

- Change API Key every 3 Months.
- Install python, "Dhan-API-Library" in python, pip, pyarrow, pandas, python.

## Code

### Dhan Basic Code

```python

from dhanhq import dhanhq
import pandas as pd

client_id = "xxxxxxxx"
access_token = "xxxxxxxxx"
dhan = dhanhq(client_id,access_token)

def get_positions(dhan):
    data_positions = dhan.get_positions()
    positions = pd.DataFrame(data_positions['data'])
    return positions
    
def get_order_list(dhan):
    data_order_list = dhan.get_order_list()
    order_list = pd.DataFrame(data_order_list['data'])
    return order_list

def get_holdings(dhan):
    data_holding = dhan.get_holdings()  
    holdings = pd.DataFrame(data_holding['data'])
    return holdings

my_holdings = get_holdings(dhan)
my_positions = get_positions(dhan)
my_order_list = get_order_list(dhan)

print(my_holdings)
print(my_order_list)
print(my_positions)
```

### 1. Import Libraries
```python
from dhanhq import dhanhq
from dhanhq import marketfeed
import pandas as pd
from datetime import datetime
```
### 2. Login code
```python
client_id = ""
access_token = ""
dhan = dhanhq(client_id,access_token)
```
### 3. Find ATM Strike
```python
''' 
- Structure for subscribing is ("exchange_segment","security_id").
- Maximum 100 instruments can be subscribed, then use 'subscribe_symbols' function.
- instruments = [(1, "1333"),(0,"13")].
- Round strike to nearest 50 to find at the money (ATM) stike '''

def atm_strike(spot_price):
    atm_strike = 0
    remainder = spot_price % 100
    if remainder < 25:
        atm_strike = int(spot_price / 100) * 100
    elif remainder <= 75:
        atm_strike = int(spot_price / 100) * 100 + 50
    else:
        atm_strike = int(spot_price / 100) * 100 + 100
    return atm_strike
```
### 4. Set Nifty LTP Manually, Use ATM & Get Label 
```python
# set nifty last traded price manually
nifty_ltp = 22100

# get "at the money" strike
atm_strike_price = atm_strike(nifty_ltp)
debit_buy_strike_ce = atm_strike_price - 100
debit_buy_strike_pe = atm_strike_price + 100

# get instrument names
def get_symbol_name(symbol, expiry, strike, strike_type):
    instrument = f'{symbol}-{expiry}-{str(strike)}-{strike_type}'
    return instrument

# set data manually to get instrument names
symbol = 'NIFTY'
expiry = 'Feb2024'
buy_strike_ce = debit_buy_strike_ce
buy_strike_pe = debit_buy_strike_pe
strike_type_CE = 'CE'
strike_type_PE = 'PE'

# get instrument symbol names to place order
instrument_CE = get_symbol_name(symbol, expiry, buy_strike_ce, strike_type_CE)
instrument_PE = get_symbol_name(symbol, expiry, buy_strike_pe, strike_type_PE)

print("CE & PE to buy strike price for the LTP is given below")
print(instrument_CE, instrument_PE)
```
### 5. Get Symbol IDs of Strikes

```python
def get_instrument_token():
    df = pd.read_csv('api-scrip-master.csv')
    data_dict = {}
    for index, row in df.iterrows():
        trading_symbol = row['SEM_TRADING_SYMBOL']
        exm_exch_id = row['SEM_EXM_EXCH_ID']
        if trading_symbol not in data_dict:
            data_dict[trading_symbol] = {}
        data_dict[trading_symbol][exm_exch_id] = row.to_dict()
    return data_dict
token_dict = get_instrument_token()
ce_id = (token_dict[instrument_CE]['NSE']['SEM_SMST_SECURITY_ID'])
pe_id = (token_dict[instrument_PE]['NSE']['SEM_SMST_SECURITY_ID'])

print("ATM symbol names for the two ATM strieks is given below")
print(f' Planned to Buy {instrument_CE} which had ID : {ce_id} & also planned to buy {instrument_CE} which had ID :  {pe_id}')
```

# Place Order Function definition

```python
def place_order(dhan, symbol, exchange, side, quantity, order_type, price):
    exchange_segment = get_exchange(dhan, exchange)
    side = get_side(dhan, side)
    order_type = get_order_type(dhan, order_type)
    order_id = dhan.place_order(security_id = symbol,  
        exchange_segment=exchange_segment,
        transaction_type=side,
        quantity=quantity,
        order_type=order_type,
        product_type=dhan.INTRA,
        price=price)
    return order_id

def get_exchange(dhan, exchange):
    if exchange == "BSE" :
        return dhan.BSE
    elif exchange == "NSE" :
        return dhan.NSE
def get_side(dhan, side):
    if side == "BUY":
        return dhan.BUY
    elif side == "SELL":
        return dhan.SELL

def get_order_type(dhan, order_type):
    if order_type == "MARKET":
        return dhan.MARKET
    elif order_type == "LIMIT":
        return dhan.LIMIT
```
### 2. Entry Logic

```python
def entry_condition():
    order_id_ce = place_order(ce_id, 'NSE', 'BUY' 50, MARKET, 0)
    order_id_pe = place_order(pe_id, 'NSE', 'BUY' 50, MARKET, 0)
    return

while True:
    curr_time = datetime.now().strftime("%H:%M:%S")
    entry_flag = 0
    if curr_time >= "11:11:00" and entry_flag == 0:
        entry_condition()
        entry_flag = 1
    if entry_flag == 1:
        break
```
### 3. Exit Logic

```python
def exit_condition():
    order_id_ce = place_order(ce_id, 'NSE', 'BUY' 50, MARKET, 0)
    order_id_pe = place_order(pe_id, 'NSE', 'BUY' 50, MARKET, 0)
    return

while True:
    curr_time = datetime.now().strftime("%H:%M:%S")
    exit_flag = 0
    if curr_time >= "11:12:00" and exit_flag == 0:
        exit_condition()
        exit_flag = 1
    if exit_flag == 1:
        break
```

### 4. Stop loss logic

### 5. Take Profit Logic

![](https://i.imgur.com/ZLbuXZym.jpeg)