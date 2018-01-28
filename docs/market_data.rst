マーケットデータエンドポイント
=====================


`マーケットデプスの取得 <binance.html#binance.client.Client.get_order_book>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    depth = client.get_order_book(symbol='BNBBTC')

`最近のトレードを取得 <binance.html#binance.client.Client.get_recent_trades>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    trades = client.get_recent_trades(symbol='BNBBTC')

`トレードのヒストリカルデータを取得 <binance.html#binance.client.Client.get_historical_trades>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    trades = client.get_historical_trades(symbol='BNBBTC')

`トレード集計を取得 <binance.html#binance.client.Client.get_aggregate_trades>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    trades = client.get_aggregate_trades(symbol='BNBBTC')


`Kline/ローソク足を取得 <binance.html#binance.client.Client.get_klines>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    candles = client.get_klines(symbol='BNBBTC', interval=Client.KLINE_INTERVAL_30MINUTE)

`Kline/ローソク足のヒストリカルデータを取得 <binance.html#binance.client.Client.get_historical_klines>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

klineの日付範囲指定・インターバル指定のデータを取得する。

.. code:: python

    # 前日から現在までの1 minute klinesを取得
    klines = client.get_historical_klines("BNBBTC", Client.KLINE_INTERVAL_1MINUTE, "1 day ago UTC")

    # klineの2017年12月のデータを取得
    klines = client.get_historical_klines("ETHBTC", Client.KLINE_INTERVAL_30MINUTE, "1 Dec, 2017", "1 Jan, 2018")

    # 上場以来のweekly klinesを取得
    klines = client.get_historical_klines("NEOBTC", KLINE_INTERVAL_1WEEK, "1 Jan, 2017")

`24hr Tickerを取得 <binance.html#binance.client.Client.get_ticker>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    tickers = client.get_ticker()

`全ての価格を取得 <binance.html#binance.client.Client.get_all_tickers>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

全てのマーケットの最新の価格を取得

.. code:: python

    prices = client.get_all_tickers()

`Orderbook Tickersを取得 <binance.html#binance.client.Client.get_orderbook_tickers>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

全てのマーケットのオーダーブックの、最初のbidとaskのエントリーを取得

.. code:: python

    tickers = client.get_orderbook_tickers()
