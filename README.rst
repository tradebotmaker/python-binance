================================
Welcome to python-binance v0.6.2
================================

.. image:: https://img.shields.io/pypi/v/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/pypi/l/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/travis/sammchardy/python-binance.svg
    :target: https://travis-ci.org/sammchardy/python-binance

.. image:: https://img.shields.io/coveralls/sammchardy/python-binance.svg
    :target: https://coveralls.io/github/sammchardy/python-binance

.. image:: https://img.shields.io/pypi/wheel/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

.. image:: https://img.shields.io/pypi/pyversions/python-binance.svg
    :target: https://pypi.python.org/pypi/python-binance

This is an unofficial Python wrapper for the `Binance exchange REST API v1/3 <https://github.com/binance-exchange/binance-official-api-docs>`_. I am in no way affiliated with Binance, use at your own risk.
これは `Binance exchange REST API v1/3 <https://github.com/binance-exchange/binance-official-api-docs>`_ の非公式のPythonラッパーです。使用に関しては、リスクを理解してご自身の責任においてご利用ください。

If you came here looking for the `Binance exchange <https://www.binance.com/?ref=21077398>`_ to purchase cryptocurrencies, then `go here <https://www.binance.com/?ref=10099792>`_. If you want to automate interactions with Binance stick around.
`Binance <https://www.binance.com/?ref=21077398>`_ で仮想通貨を通常の手続きで購入するには、 `こちら<https://www.binance.com/?ref=21077398>`_ からご利用ください。仮想通貨取引を自動化したい場合は、引き続きこのドキュメントをお読みください。

Source code
  https://github.com/sammchardy/python-binance

Documentation
  https://python-binance-jp.readthedocs.io/ja/latest/

Binance API Telegram
  https://t.me/binance_api_english

Blog with examples
  https://sammchardy.github.io

Make sure you update often and check the `Changelog <https://python-binance.readthedocs.io/en/latest/changelog.html>`_ for new features and bug fixes.
定期的に `Changelog <https://python-binance.readthedocs.io/en/latest/changelog.html>`_ を確認し、新機能の追加やバグ修正がないかをチェックしてください。

Features　主な機能
--------

- Implementation of all General, Market Data and Account endpoints.  全ての通常機能、マーケットデータ、口座のエンドポイントの実装
- Simple handling of authentication　使いやすい認証処理
- No need to generate timestamps yourself, the wrapper does it for you タイムスタンプを自分で生成する必要はありません。ラッパーが行います。
- Response exception handling　レスポンスのエラーハンドリング
- Websocket handling with reconnection and multiplexed connections　再接続機能付きのウェブソケットの接続処理と複数接続
- Symbol Depth Cache　通貨ペアのデプスキャッシュ
- Historical Kline/Candle fetching function　Kline/ローソク足のヒストリカルデータ取得関数
- Withdraw functionality 出金機能
- Deposit addresses　デポジットアドレス

Quick Start　クイックスタート
-----------

`Register an account with Binance <https://www.binance.com/?ref=21077398>`_.
`Binanceで口座開設 <https://www.binance.com/?ref=21077398>`_.


`Generate an API Key <https://www.binance.com/userCenter/createApi.html>`_ and assign relevant permissions.
`API Keyの生成 <https://www.binance.com/userCenter/createApi.html>`_ と許可設定


.. code:: bash

    pip install python-binance


.. code:: python

    from binance.client import Client
    client = Client(api_key, api_secret)

    # get market depth　マーケットデプスの取得
    depth = client.get_order_book(symbol='BNBBTC')

    # place a test market buy order, to place an actual order use the create_order function　テスト成行注文　実際の注文を送信するには、create_order関数を使用
    order = client.create_test_order(
        symbol='BNBBTC',
        side=Client.SIDE_BUY,
        type=Client.ORDER_TYPE_MARKET,
        quantity=100)

    # get all symbol prices 全ての仮装通貨ペアの価格取得
    prices = client.get_all_tickers()

    # withdraw 100 ETH　100 ETH出金
    # check docs for assumptions around withdrawals　出金に関しては、docs参照
    from binance.exceptions import BinanceAPIException, BinanceWithdrawException
    try:
        result = client.withdraw(
            asset='ETH',
            address='<eth_address>',
            amount=100)
    except BinanceAPIException as e:
        print(e)
    except BinanceWithdrawException as e:
        print(e)
    else:
        print("Success")

    # fetch list of withdrawals 出金リストの取得
    withdraws = client.get_withdraw_history()

    # fetch list of ETH withdrawals ETH出金のリストを取得
    eth_withdraws = client.get_withdraw_history(asset='ETH')

    # get a deposit address for BTC BTCのデポジットアドレスを取得
    address = client.get_deposit_address(asset='BTC')

    # start aggregated trade websocket for BNBBTC　BNBBTC用のトレードウェブソケットの開始
    def process_message(msg):
        print("message type: {}".format(msg['e']))
        print(msg)
        # do something

    from binance.websockets import BinanceSocketManager
    bm = BinanceSocketManager(client)
    bm.start_aggtrade_socket('BNBBTC', process_message)
    bm.start()

    # get historical kline data from any date range　任意の日付範囲のklineヒストリカルデータの取得

    # fetch 1 minute klines for the last day up until now　前日から今までの1 minute klinesを取得
    klines = client.get_historical_klines("BNBBTC", Client.KLINE_INTERVAL_1MINUTE, "1 day ago UTC")

    # fetch 30 minute klines for the last month of 2017　2017年12月の30 minute klinesを取得
    klines = client.get_historical_klines("ETHBTC", Client.KLINE_INTERVAL_30MINUTE, "1 Dec, 2017", "1 Jan, 2018")

    # fetch weekly klines since it listed 上場からのweekly klinesを取得
    klines = client.get_historical_klines("NEOBTC", KLINE_INTERVAL_1WEEK, "1 Jan, 2017")

For more `check out the documentation <https://python-binance.readthedocs.io/en/latest/>`_.
詳細は、`ドキュメント <https://python-binance.readthedocs.io/en/latest/>`_ をお読みください。
