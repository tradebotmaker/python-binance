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

これは `Binance exchange REST API v1/3 <https://github.com/binance-exchange/binance-official-api-docs>`_ の非公式のPythonラッパーです。使用に関しては、リスクを理解してご自身の責任においてご利用ください。

`Binance <https://www.binance.com/?ref=21077398>`_ で仮想通貨を通常の手続きで購入するには、 `こちら <https://www.binance.com/?ref=21077398>`_ からご利用ください。仮想通貨取引を自動化したい場合は、引き続きこのドキュメントをお読みください。

Source code
  https://github.com/sammchardy/python-binance

Documentation
  https://python-binance-jp.readthedocs.io/ja/latest/

Binance API Telegram
  https://t.me/binance_api_english

Blog with examples
  https://sammchardy.github.io

定期的に `Changelog <https://python-binance.readthedocs.io/en/latest/changelog.html>`_ を確認し、新機能の追加やバグ修正がないかをチェックしてください。

主な機能
--------

- 全ての通常機能、マーケットデータ、口座のエンドポイントの実装
- 使いやすい認証処理
- タイムスタンプを自分で生成する必要はありません。ラッパーが行います。
- レスポンスのエラーハンドリング
- 再接続機能付きのウェブソケットの接続処理と複数接続
- 通貨ペアのデプスキャッシュ
- Kline/ローソク足のヒストリカルデータ取得関数
- 出金機能
- デポジットアドレス

クイックスタート
-----------

`Binanceで口座開設 <https://www.binance.com/?ref=21077398>`_.


`API Keyの生成 <https://www.binance.com/userCenter/createApi.html>`_ と許可設定


.. code:: bash

    pip install python-binance


.. code:: python

    from binance.client import Client
    client = Client(api_key, api_secret)

    # マーケットデプスの取得
    depth = client.get_order_book(symbol='BNBBTC')

    # テスト成行注文　実際の注文を送信するには、create_order関数を使用
    order = client.create_test_order(
        symbol='BNBBTC',
        side=Client.SIDE_BUY,
        type=Client.ORDER_TYPE_MARKET,
        quantity=100)

    # 全ての仮想通貨ペアの価格取得
    prices = client.get_all_tickers()

    # 100 ETH出金
    # 出金に関しては、docs参照
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

    # 出金履歴リストの取得
    withdraws = client.get_withdraw_history()

    # ETH出金のリストを取得
    eth_withdraws = client.get_withdraw_history(asset='ETH')

    # BTCのデポジットアドレスを取得
    address = client.get_deposit_address(asset='BTC')

    # BNBBTC用のトレードウェブソケットの開始
    def process_message(msg):
        print("message type: {}".format(msg['e']))
        print(msg)
        # 処理を記述

    from binance.websockets import BinanceSocketManager
    bm = BinanceSocketManager(client)
    bm.start_aggtrade_socket('BNBBTC', process_message)
    bm.start()

    # 任意の日付範囲のklineヒストリカルデータの取得

    # 前日から今までの1 minute klinesを取得
    klines = client.get_historical_klines("BNBBTC", Client.KLINE_INTERVAL_1MINUTE, "1 day ago UTC")

    # 2017年12月の30 minute klinesを取得
    klines = client.get_historical_klines("ETHBTC", Client.KLINE_INTERVAL_30MINUTE, "1 Dec, 2017", "1 Jan, 2018")

    # 上場からのweekly klinesを取得
    klines = client.get_historical_klines("NEOBTC", KLINE_INTERVAL_1WEEK, "1 Jan, 2017")


詳細は、 `ドキュメント <https://python-binance.readthedocs.io/en/latest/>`_ をお読みください。
