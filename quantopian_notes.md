# Investing on Quantopian  

Trading algorithm on Quantopian has: 
1. initialize(context) function, called only once when the program starts to perform one-time startup logic 
2. handle_data(context, data), called once at the end of every minute in 'bars' in simulation or live trading to decide which orders should be placed each minute
3. before_trading_start(context, data), called once per day before market opens, used when selecting stocks to buy. 

"context" is a Python dictionary for maintaining states during backtesting or live trading, use this instead of global variables, refer to properties with dot notation, context.some_property. 	
"data" stores API functions. 

````
def initialize(context):
    # Reference to AAPL
    context.aapl = sid(24)

def handle_data(context, data):
    # Position 100% of our portfolio to be long in AAPL
    order_target_percent(context.aapl, 1.00)
````  
This code allocates its full portfolio into Apple stock. 

We use pipelines to dynamically trade securities, for now we manually reference hand picked stocks by using sid() and symbol(). 

1. sid() unique integer security ID that references a security, robust way to reference a security because it never changes (even if ticker does). Store reference to a particular stock like: 
````
context.aapl = sid(24)
````  
2. Can use symbol() but must set a reference data with set_symbol_lookup_date('YYYY-MM-DD'). sid() is easier. 

Ordering Securities 

1. order_target_percentage(asset being ordered, target percentage). Following line orders 50% of portfolio value as AAPL:   
````
order_target_percent(sid(24), 0.50)
````  
To do a short position, enter a negative target value. 

Short position- profits when a share drops in price, bearish stock
Long position- profits when a share raises in price, when investors expect growth/bullish stock

The following code takes a long position on Apple to be 60% of our portfolio, and short on SPY ETF worth 40% of our value: 


````
def initialize(context):
    context.aapl = sid(24)
    context.spy = sid(8554)

def handle_data(context, data):
    # Note: data.can_trade() is explained in the next lesson
    if data.can_trade(context.aapl):
        order_target_percent(context.aapl, 0.60)
    if data.can_trade(context.spy):
        order_target_percent(context.spy, -0.40)
````
- Get the most recent price of a stock: 
````
data.current(sid(24), 'price')
data.current([sid(24), sid(8554)], 'price') # price of both in pandas series 
data.current([sid(24), sid(8554)], ['low', 'high']) # last known low and high prices of both stocks, pandas dataframe 
````
data.can_trade() returns true if the share is on a supported major exchange and can be ordered, can change in each "bar" minute, can order the asset in that bar if true. 
````
data.can_trade(sid(24)) 
````
