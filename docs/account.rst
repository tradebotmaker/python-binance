アカウントエンドポイント
=================

注文
------

注文バリデーション
^^^^^^^^^^^^^^^^

Binanceには、通貨ペアの注文に対して、最低価格、数、総注文価格についてのルールがあります。

詳細は、 `Filters <https://github.com/binance-exchange/binance-official-api-docs/blob/master/rest-api.md#filters>`_ のオフィシャルAPIのセクションを参照してください。


下記のスニペットを参考に、出力の整形をしてください。

.. code:: python

    amount = 0.000234234
    precision = 5
    amt_str = "{:0.0{}f}".format(amount, precision)


`全注文の読み込み <binance.html#binance.client.Client.get_all_orders>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    orders = client.get_all_orders(symbol='BNBBTC', limit=10)


`注文送信 <binance.html#binance.client.Client.create_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


**注文送信**

注文の作成は、 `create_order` 関数を使用してください。

.. code:: python

    from binance.enums import *
    order = client.create_order(
        symbol='BNBBTC',
        side=SIDE_BUY,
        type=ORDER_TYPE_LIMIT,
        timeInForce=TIME_IN_FORCE_GTC,
        quantity=100,
        price='0.00001')

**指値注文の送信**

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


**成行注文**

成行買いまたは売りの注文をするにはヘルパー関数を使用してください。

.. code:: python

    order = client.order_market_buy(
        symbol='BNBBTC',
        quantity=100)

    order = client.order_market_sell(
        symbol='BNBBTC',
        quantity=100)

`テスト注文の送信 <binance.html#binance.client.Client.create_test_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


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

`注文状況確認 <binance.html#binance.client.Client.get_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    order = client.get_order(
        symbol='BNBBTC',
        orderId='orderId')


`注文キャンセル <binance.html#binance.client.Client.cancel_order>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    result = client.cancel_order(
        symbol='BNBBTC',
        orderId='orderId')


`全てのオープン注文の取得 <binance.html#binance.client.Client.get_open_orders>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    orders = client.get_open_orders(symbol='BNBBTC')

`全ての注文の取得 <binance.html#binance.client.Client.get_all_orders>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    orders = client.get_all_orders(symbol='BNBBTC')


アカウント
----------

`アカウント情報の取得 <binance.html#binance.client.Client.get_account>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    info = client.get_account()

`口座残高の取得 <binance.html#binance.client.Client.get_asset_balance>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    balance = client.get_asset_balance(asset='BTC')

`口座状況の取得 <binance.html#binance.client.Client.get_account_status>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. code:: python

    status = client.get_account_status()

`取引情報の取得 <binance.html#binance.client.Client.get_my_trades>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    trades = client.get_my_trades(symbol='BNBBTC')
