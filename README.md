import ccxt

binance_api = {
    "apiKey": "your_binance_api_key",
    "secret": "your_binance_api_secret"
}
bybit_api = {
    "apiKey": "your_bybit_api_key",
    "secret": "your_bybit_api_secret"
}

binance = ccxt.binance(binance_api)
bybit = ccxt.bybit(bybit_api)

symbol = "BTC/USDT"
amount = 0.01

binance_ticker = binance.fetch_ticker(symbol)
bybit_ticker = bybit.fetch_ticker(symbol)

binance_price = binance_ticker['last']
bybit_price = bybit_ticker['last']

print(f"Binance price: {binance_price}, Bybit price: {bybit_price}")

if binance_price > bybit_price:
    print("Buying on Bybit, selling on Binance")
    buy_order = bybit.create_market_buy_order(symbol, amount)
    sell_order = binance.create_market_sell_order(symbol, amount)
else:
    print("Buying on Binance, selling on Bybit")
    buy_order = binance.create_market_buy_order(symbol, amount)
    sell_order = bybit.create_market_sell_order(symbol, amount)

print("Orders executed")

