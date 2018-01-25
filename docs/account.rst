Account Endpoints アカウントエンドポイント
=================

Orders 注文
------

Order Validation 注文バリデーション
^^^^^^^^^^^^^^^^

Binance has a number of rules around symbol pair orders with validation on minimum price, quantity and total order value.
Binanceには、通貨ペアの注文に対して、最低価格、数、総注文価格についてのルールがあります。

Read more about their specifics in the `Filters <https://github.com/binance-exchange/binance-official-api-docs/blob/master/rest-api.md#filters>`_
section of the official API.
詳細は、`Filters <https://github.com/binance-exchange/binance-official-api-docs/blob/master/rest-api.md#filters>`_のofficial APIのセクションを参照してください。


It can be helpful to format the output using the following snippet
下記のスニペットを参考に、出力の整形をしてください。

.. code:: python

    amount = 0.000234234
    precision = 5
    amt_str = "{:0.0{}f}".format(amount, precision)


`Fetch all orders <binance.html#binance.client.Client.get_all_orders>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`全注文の読み込み <binance.html#binance.client.Client.get_all_orders>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    orders = client.get_all_orders(symbol='BNBBTC', limit=10)


`Place an order <binance.html#binance.client.Client.create_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`注文送信 <binance.html#binance.client.Client.create_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


**Place an order　注文送信**

Use the `create_order` function to have full control over creating an order
注文の作成は、`create_order`関数を使用してください。

.. code:: python

    from binance.enums import *
    order = client.create_order(
        symbol='BNBBTC',
        side=SIDE_BUY,
        type=ORDER_TYPE_LIMIT,
        timeInForce=TIME_IN_FORCE_GTC,
        quantity=100,
        price='0.00001')

**Place a limit order　指値注文の送信**

Use the helper functions to easily place a limit buy or sell order
指値買いまたは売りの注文をするにはヘルパー関数を使用してください。

.. code:: python

    order = client.order_limit_buy(
        symbol='BNBBTC',
        quantity=100,
        price='0.00001')

    order = client.order_limit_sell(
        symbol='BNBBTC',
        quantity=100,
        price='0.00001')


**Place a market order　成行注文**

Use the helper functions to easily place a market buy or sell order
成行買いまたは売りの注文をするにはヘルパー関数を使用してください。

.. code:: python

    order = client.order_market_buy(
        symbol='BNBBTC',
        quantity=100)

    order = client.order_market_sell(
        symbol='BNBBTC',
        quantity=100)

`Place a test order <binance.html#binance.client.Client.create_test_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`テスト注文の送信 <binance.html#binance.client.Client.create_test_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Creates and validates a new order but does not send it into the exchange.
新規注文を作成し、バリデートしますが、取引所には送信しません。

.. code:: python

    from binance.enums import *
    order = client.create_test_order(
        symbol='BNBBTC',
        side=SIDE_BUY,
        type=ORDER_TYPE_LIMIT,
        timeInForce=TIME_IN_FORCE_GTC,
        quantity=100,
        price='0.00001')

`Check order status <binance.html#binance.client.Client.get_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`注文状況確認 <binance.html#binance.client.Client.get_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    order = client.get_order(
        symbol='BNBBTC',
        orderId='orderId')


`Cancel an order <binance.html#binance.client.Client.cancel_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`注文キャンセル <binance.html#binance.client.Client.cancel_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    result = client.cancel_order(
        symbol='BNBBTC',
        orderId='orderId')


`Get all open orders <binance.html#binance.client.Client.get_open_orders>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`全てのオープン注文の取得 <binance.html#binance.client.Client.get_open_orders>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    orders = client.get_open_orders(symbol='BNBBTC')

`Get all orders <binance.html#binance.client.Client.get_all_orders>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`全ての注文の取得 <binance.html#binance.client.Client.get_all_orders>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    orders = client.get_all_orders(symbol='BNBBTC')


Account
-------

`Get account info <binance.html#binance.client.Client.get_account>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`アカウント情報の取得 <binance.html#binance.client.Client.get_account>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    info = client.get_account()

`Get asset balance <binance.html#binance.client.Client.get_asset_balance>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`口座残高の取得 <binance.html#binance.client.Client.get_asset_balance>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    balance = client.get_asset_balance(asset='BTC')

`Get account status <binance.html#binance.client.Client.get_account_status>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`口座状況の取得 <binance.html#binance.client.Client.get_account_status>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. code:: python

    status = client.get_account_status()

`Get trades <binance.html#binance.client.Client.get_my_trades>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`取引情報の取得 <binance.html#binance.client.Client.get_my_trades>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    trades = client.get_my_trades(symbol='BNBBTC')
