Websockets　ウェブソケット
==========

Sockets are handled through a Socket Manager ソケットはソケットマネージャーにより処理されます。`BinanceSocketManager <binance.html#binance.websockets.BinanceSocketManager>`_.

Multiple socket connections can be made through the manager.
マネージャーを通して複数のソケット接続をすることができます。

Only one instance of each socket type will be created, i.e. only one BNBBTC Depth socket can be created
and there can be both a BNBBTC Depth and a BNBBTC Trade socket open at once.
それぞれのソケットタイプで一つだけインスタンスが作成されます。例えば、一つのBNBBTC Depth socketを作成し、これはBNBBTC DepthとBNBBTC Tradeの両方のソケットとして同時に使用できます。

When creating socket connections a callback function is passed which receives the messages.
ソケット接続を作成すると、コールバック関数はメッセージを受信します。

Messages are received as dictionary objects relating to the message formats defined in the `Binance WebSocket API documentation <https://github.com/binance-exchange/binance-official-api-docs/blob/master/web-socket-streams.md>`_.
メッセージはディクショナリーオブジェクトとして受信されます。フォーマットは、 `Binance WebSocket API documentation <https://github.com/binance-exchange/binance-official-api-docs/blob/master/web-socket-streams.md>`_を参照してください。

Websockets are setup to reconnect with a maximum of 5 retries.
ウェブソケットは最大５回まで再接続にリトライする設定をすることができます。

Websocket Usage　ウェブソケットの使用方法
---------------

Create the manager like so, passing the API client.
マネージャーを作成し、API clientを渡します。

.. code:: python

    from binance.websockets import BinanceSocketManager
    bm = BinanceSocketManager(client)
    # start any sockets here, i.e a trade socket 任意のソケットを開始　例）trade socket
    conn_key = bm.start_trade_socket('BNBBTC', process_message)
    # then start the socket manager ソケットマネージャーを開始
    bm.start()

A callback to process messages would take the format
メッセージを処理するコールバック

.. code:: python

    def process_message(msg):
        print("message type: {}".format(msg['e']))
        print(msg)
        # do something


Websocket Errors ウェブソケットエラー
----------------

If the websocket is disconnected and is unable to reconnect a message is sent to the callback to indicate this. The format is
ウェブソケットが切断され、再接続ができなかった場合、このことを知らせるメッセージがコールバックに送られます。このフォーマットは下記の通りです。

.. code:: python

    {
        'e': 'error',
        'm': 'Max reconnect retries reached'
    }

    # check for it like so　メッセージの確認
    def process_message(msg):
        if msg['e'] == 'error':
            # close and restart the socket　閉じるかソケットを再スタートする処理
        else:
            # process message normally　通常のメッセージ処理


`Multiplex Socket マルチプレックスソケット<binance.html#binance.websockets.BinanceSocketManager.start_multiplex_socket>`_
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Create a socket combining multiple streams. 複数のストリームを合わせたソケットを作成します。

These streams can include the depth, kline, ticker and trade streams but not the user stream which requires extra authentication.
これらのストリームには、depth, kline, ticker, tradeを含めることができます。ただし、ユーザーストリームは別の認証が必要なため、含めることはできません。

Symbols in socket name must be lowercase i.e 
ソケット名に使用するSymbolは、小文字でなければなりません。　例）bnbbtc@aggTrade, neobtc@ticker

See the `Binance Websocket Streams API documentation <https://github.com/binance-exchange/binance-official-api-docs/blob/master/web-socket-streams.md>`_ for details on socket names.
ソケット名についての詳細は、`Binance Websocket Streams API documentation <https://github.com/binance-exchange/binance-official-api-docs/blob/master/web-socket-streams.md>`_を参照してください。

.. code:: python

    def process_m_message(msg):
        print("stream: {} data: {}".format(msg['stream'], msg['data']))

    # pass a list of stream names ストリーム名のリストを渡す
    conn_key = bm.start_multiplex_socket(['bnbbtc@aggTrade', 'neobtc@ticker'], process_m_message)

`Depth Socket <binance.html#binance.websockets.BinanceSocketManager.start_depth_socket>`_
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Depth sockets have an optional depth parameter to receive partial book rather than a diff response.
By default this the diff response is returned.
Valid depth values are 5, 10 and 20 and `defined as enums <enums.html>`_.
デプスソケットには、diff responseではなく、partial bookを受信するためのオプションのデプスパラメーターがあります。

.. code:: python

    # depth diff response
    diff_key = bm.start_depth_socket('BNBBTC', process_message)

    # partial book response
    partial_key = bm.start_depth_socket('BNBBTC', process_message, depth=BinanceSocketManager.WEBSOCKET_DEPTH_5)


`Kline Socket <binance.html#binance.websockets.BinanceSocketManager.start_kline_socket>`_
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Kline sockets have an optional interval parameter. By default this is set to 1 minute.
Valid interval values are `defined as enums <enums.html>`_.
Klineソケットには、オプションのインターバルパラメータがあります。デフォルトでは1 minuteに設定されています。
有効なインターバル値については、`defined as enums <enums.html>`_を参照してください。

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

This watches for 3 different user events
これは、３つの異なったユーザーイベントを監視します。

- Account Update Event
- Order Update Event
- Trade Update Event

The Manager handles keeping the socket alive.
マネージャーはソケットが接続され続けるようにします。

.. code:: python

    bm.start_user_socket(process_message)


`Close a Socket 　ソケットの停止<binance.html#binance.websockets.BinanceSocketManager.stop_socket>`_
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

To close an individual socket call the `stop_socket` function.
This takes a conn_key parameter which is returned when starting the socket.
個別のソケットを停止するには、`stop_socket`関数を呼びます。
この関数は開始時にリターンされるconn_key parameterを使用します。

.. code:: python

    bm.stop_socket(conn_key)


To stop all sockets and end the manager call `close` after doing this a `start` call would be required to connect any new sockets.
全てのソケットを停止し、マネージャーを終了するには、`close`を使用します。その後に新しいソケットに接続するには、`start`を使用する必要があります。

.. code:: python

    bm.close()

.. image:: https://analytics-pixel.appspot.com/UA-111417213-1/github/python-binance/docs/websockets?pixel


Close and exit program　停止とプログラムの終了
++++++++++++++++++++++

Websockets utilise a reactor loop from the Twisted library. Using the `close` method above will close
the websocket connections but it won't stop the reactor loop so your code may not exit when you expect.
ウェブソケットはTwistedライブラリからのリアクターループを使用します。上記の`close`メソッドを使用するとウェブソケット接続を停止しますが、リアクターループは停止しません。コードは意図したタイミングで終了しないかもしれません。

If you do want to exit then use the `stop` method from reactor like below.
終了させたい場合、下記のようにリアクターの`stop`メソッドを使用します。

.. code:: python

    from twisted.internet import reactor

    # program code here　プログラムコード

    # when you need to exit　終了時
    reactor.stop()
