import ccxt

def initialize_exchange(api_key, secret, exchange_class):
    return exchange_class({"apiKey": api_key, "secret": secret, "enableRateLimit": True})

def get_price(exchange, symbol):
    try:
        return exchange.fetch_ticker(symbol)['last']
    except Exception as e:
        print(f"Error fetching price from {exchange.id}: {e}")
        return None

def execute_hedged_trade(buy_exchange, sell_exchange, symbol, amount):
    try:
        print(f"Buying on {buy_exchange.id}, selling on {sell_exchange.id}")
        buy_order = buy_exchange.create_market_buy_order(symbol, amount)
        sell_order = sell_exchange.create_market_sell_order(symbol, amount)
        print("Orders executed successfully")
        return buy_order, sell_order
    except Exception as e:
        print(f"Error executing trade: {e}")
        return None, None

binance = initialize_exchange("your_binance_api_key", "your_binance_api_secret", ccxt.binance)
bybit = initialize_exchange("your_bybit_api_key", "your_bybit_api_secret", ccxt.bybit)

symbol = "BTC/USDT"
amount = 0.01

binance_price = get_price(binance, symbol)
bybit_price = get_price(bybit, symbol)

if binance_price is None or bybit_price is None:
    print("Failed to fetch prices. Exiting...")
else:
    print(f"Binance price: {binance_price}, Bybit price: {bybit_price}")
    if binance_price > bybit_price:
        execute_hedged_trade(bybit, binance, symbol, amount)
    else:
        execute_hedged_trade(binance, bybit, symbol, amount)
