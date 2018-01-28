一般的なエンドポイント
=================

`サーバーへPing送信 <binance.html#binance.client.Client.ping>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    client.ping()

`サーバー時間を取得 <binance.html#binance.client.Client.get_server_time>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    time_res = client.get_server_time()

`取引所情報を取得 <binance.html#binance.client.Client.get_exchange_info>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    info = client.get_exchange_info()

`Symbol（通貨ペア）情報を取得 <binance.html#binance.client.Client.get_symbol_info>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

特定のSymbolの取引所情報を取得する。

.. code:: python

    info = client.get_symbol_info('BNBBTC')

`現在のProductsを取得 <binance.html#binance.client.Client.get_products>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

このコールは非推奨となりました。上記の取引所情報のコールを使用してください。

.. code:: python

    products = client.get_products()
