ウェブソケット
==========

ソケットはソケットマネージャーにより処理されます。 `BinanceSocketManager <binance.html#binance.websockets.BinanceSocketManager>`_

マネージャーを通して複数のソケット接続をすることができます。

それぞれのソケットタイプで一つだけインスタンスが作成されます。例えば、一つのBNBBTC Depth socketを作成し、これはBNBBTC DepthとBNBBTC Tradeの両方のソケットとして同時に使用できます。

ソケット接続を作成すると、コールバック関数はメッセージを受信します。

メッセージはディクショナリーオブジェクトとして受信されます。フォーマットは、 `Binance WebSocket API documentation <https://github.com/binance-exchange/binance-official-api-docs/blob/master/web-socket-streams.md>`_ を参照してください。

ウェブソケットは最大５回まで再接続にリトライする設定をすることができます。

ウェブソケットの使用方法
---------------

マネージャーを作成し、API clientを渡します。

.. code:: python

    from binance.websockets import BinanceSocketManager
    bm = BinanceSocketManager(client)
    # 任意のソケットを開始　例）trade socket
    conn_key = bm.start_trade_socket('BNBBTC', process_message)
    # ソケットマネージャーを開始
    bm.start()

メッセージを処理するコールバック

.. code:: python

    def process_message(msg):
        print("message type: {}".format(msg['e']))
        print(msg)
        # 処理を記述


ウェブソケットエラー
----------------

ウェブソケットが切断され、再接続ができなかった場合、このことを知らせるメッセージがコールバックに送られます。このフォーマットは下記の通りです。

.. code:: python

    {
        'e': 'error',
        'm': 'Max reconnect retries reached'
    }

    # メッセージの確認
    def process_message(msg):
        if msg['e'] == 'error':
            # 閉じるかソケットを再スタートする処理
        else:
            # 通常のメッセージ処理


`マルチプレックスソケット <binance.html#binance.websockets.BinanceSocketManager.start_multiplex_socket>`_
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

複数のストリームを合わせたソケットを作成します。

これらのストリームには、depth, kline, ticker, tradeを含めることができます。ただし、ユーザーストリームは別の認証が必要なため、含めることはできません。

ソケット名に使用するSymbolは、小文字でなければなりません。　例）bnbbtc@aggTrade, neobtc@ticker

ソケット名についての詳細は、 `Binance Websocket Streams API documentation  <https://github.com/binance-exchange/binance-official-api-docs/blob/master/web-socket-streams.md>`_ を参照してください。

.. code:: python

    def process_m_message(msg):
        print("stream: {} data: {}".format(msg['stream'], msg['data']))

    # ストリーム名のリストを渡す
    conn_key = bm.start_multiplex_socket(['bnbbtc@aggTrade', 'neobtc@ticker'], process_m_message)

`Depth Socket <binance.html#binance.websockets.BinanceSocketManager.start_depth_socket>`_
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

デプスソケットには、diff responseではなく、partial bookを受信するためのオプションのデプスパラメーターがあります。
デフォルトではdiff responseが返されます。
有効なデプス値は、5, 10, 20と `列挙型で定義 <enums.html>`_ されている値です。

.. code:: python

    # depth diff response
    diff_key = bm.start_depth_socket('BNBBTC', process_message)

    # partial book response
    partial_key = bm.start_depth_socket('BNBBTC', process_message, depth=BinanceSocketManager.WEBSOCKET_DEPTH_5)


`Kline Socket <binance.html#binance.websockets.BinanceSocketManager.start_kline_socket>`_
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Klineソケットには、オプションのインターバルパラメータがあります。デフォルトでは1 minuteに設定されています。
有効なインターバル値については、 `列挙型定義 <enums.html>`_ を参照してください。

.. code:: python

    from binance.enums import *
    conn_key = bm.start_kline_socket('BNBBTC', process_message, interval=KLINE_INTERVAL_30MINUTE)


`Aggregated Trade Socket <binance.html#binance.websockets.BinanceSocketManager.start_aggtrade_socket>`_
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code:: python

    conn_key = bm.start_aggtrade_socket('BNBBTC', process_message)


`Trade Socket <binance.html#binance.websockets.BinanceSocketManager.start_trade_socket>`_
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code:: python

    conn_key = bm.start_trade_socket('BNBBTC', process_message)

`Symbol Ticker Socket <binance.html#binance.websockets.BinanceSocketManager.start_symbol_ticker_socket>`_
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code:: python

    conn_key = bm.start_symbol_ticker_socket('BNBBTC', process_message)

`Ticker Socket <binance.html#binance.websockets.BinanceSocketManager.start_ticker_socket>`_
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. code:: python

    conn_key = bm.start_ticker_socket(process_message)

`User Socket <binance.html#binance.websockets.BinanceSocketManager.start_user_socket>`_
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

これは、３つの異なったユーザーイベントを監視します。

- Account Update Event
- Order Update Event
- Trade Update Event

マネージャーはソケットが接続され続けるようにします。

.. code:: python

    bm.start_user_socket(process_message)


`ソケットの停止 <binance.html#binance.websockets.BinanceSocketManager.stop_socket>`_
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

個別のソケットを停止するには、 `stop_socket` 関数を呼びます。
この関数は開始時にリターンされるconn_key parameterを使用します。

.. code:: python

    bm.stop_socket(conn_key)


全てのソケットを停止し、マネージャーを終了するには、 `close` を使用します。その後に新しいソケットに接続するには、 `start` を使用する必要があります。

.. code:: python

    bm.close()

.. image:: https://analytics-pixel.appspot.com/UA-111417213-1/github/python-binance/docs/websockets?pixel


停止とプログラムの終了
++++++++++++++++++++++

ウェブソケットはTwistedライブラリからのリアクターループを使用します。上記の `close` メソッドを使用するとウェブソケット接続を停止しますが、リアクターループは停止しません。コードは意図したタイミングで終了しないかもしれません。

終了させたい場合、下記のようにリアクターの `stop` メソッドを使用します。

.. code:: python

    from twisted.internet import reactor

    # プログラムコード

    # 終了時
    reactor.stop()
