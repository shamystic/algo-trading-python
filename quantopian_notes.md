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
To do a short position, enter a negative target value 
Short position- profits when a share drops in price, bearish stock
Long position- profits when a share raises in price, when investors expect growth/bullish stock