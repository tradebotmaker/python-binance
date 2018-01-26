General Endpoints　一般的なエンドポイント
=================

`Ping the server サーバーへPing送信<binance.html#binance.client.Client.ping>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    client.ping()

`Get the server time サーバー時間を取得<binance.html#binance.client.Client.get_server_time>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    time_res = client.get_server_time()

`Get Exchange Info 取引所情報を取得<binance.html#binance.client.Client.get_exchange_info>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    info = client.get_exchange_info()

`Get Symbol Info Symbol（通貨ペア）情報を取得<binance.html#binance.client.Client.get_symbol_info>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Get the exchange info for a particular symbol
特定のSymbolの取引所情報を取得する。

.. code:: python

    info = client.get_symbol_info('BNBBTC')

`Get Current Products 現在のProductsを取得<binance.html#binance.client.Client.get_products>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This call is deprecated, use the above Exchange Info call
このコールは非推奨となりました。上記の取引所情報のコールを使用してください。

.. code:: python

    products = client.get_products()
