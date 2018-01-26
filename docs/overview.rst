はじめに
========================

インストール
-------------------------

``english test`` and `english <https:://www.test2.com>`_
``english 日本語 test`` and `english　日本語 <https:://www.test2.com>`_

``python-binance`` は `e 日本語 <https://pypi.python.org/pypi/python-binance/>`_ 確認

``python-binance`` は `_日本語 <https://pypi.python.org/pypi/python-binance/>`_ 確認


``python-binance`` は `日本語 <https://pypi.python.org/pypi/python-binance/>`_ 確認


``python-binance`` は ``pip`` により `PYPI <https://pypi.python.org/pypi/python-binance/>`_ からインストールできます。

.. code:: bash

    pip install python-binance

**Windows**

ビルドエラーが発生する場合、Microsoft Visual C++が必要です。 `Python Wiki on Widows Compilers<https://wiki.python.org/moin/WindowsCompilers>`_ を参照して、Visual C++ Build Toolsをインストールしてください。

Binanceでの登録
----------------------------------------------

まず最初に、`Binanceで登録<https://www.binance.com/?ref=21077398>`_ してください。

API Keyの取得
---------------------------------

登録した口座でプログラムを使用するには、`API Keyを作成<https://www.binance.com/userCenter/createApi.html>`_する必要があります。

クライアントの初期化
-------------------------------------------

API KeyとSecretを設定します。

.. code:: python

    from binance.client import Client
    client = Client(api_key, api_secret)

APIの呼び出し
------------------------------

`Binance API documentation<https://github.com/binance-exchange/binance-official-api-docs>`_で設定されているキーワードと同じ単語を使用した各メソッドは、任意のパラメータを対応したエンドポイントに渡します。

`Binance API documentation<https://github.com/binance-exchange/binance-official-api-docs>`_に記載されている通り、各メソッドは、JSONレスポンスのディクショナリーを返します。
各メソッドのコードのdocstringは、元となったエンドポイントの内容を参照しています。

Binance API documentationは、`timestamp`パラメータを参照しますが、要求される場合は生成されます。

メソッドによっては`recvWindow`パラメータがあります。`タイミングセキュリティーのために使用されます。詳細はBinance documentationを参照してください。<https://github.com/binance-exchange/binance-official-api-docs/blob/master/rest-api.md#timing-security>`_

APIエンドポイントのレートリミットは、Binanceにより秒間20リクエストに制限されています。制限を解除したい場合は、直接お問い合わせください。

APIレートリミット
--------------

`get_exchange_info()<binance.html#binance.client.Client.get_exchange_info>`_を確認して、最新のレートリミットに関する情報を入手してください。

現時点でのBinanceのレートリミット：

- １分間に1200リクエスト
- １秒間に10の注文
- 24時間に100,000の注文

呼び出すメソッドによっては、全ての通貨ペアの情報を読み込む場合など、他のメソッドよりも負荷がかかる場合があります。
詳細は、`official Binance documentation<https://github.com/binance-exchange/binance-official-api-docs`_ をご確認ください。

.. image:: https://analytics-pixel.appspot.com/UA-111417213-1/github/python-binance/docs/overview?pixel

Requestの設定
--------------------------------

`python-binance` は、 `requests<http://docs.python-requests.org/en/master/>`_ ライブラリを使用します。

クライアントを作成後、全てのAPIコールに対し、カスタムリクエストパラメータを設定できます。

.. code:: python

    client = Client("api-key", "api-secret", {"verify": False, "timeout": 20})

どのAPIコールでも、デフォルト設定をオーバーライドまたは再設定することにより、カスタムリクエストパラメータを送信することができます。

.. code:: python

    # get_all_ordersのコールはverify: False and timeout: 5 の結果になることがあります。
    client = Client("api-key", "api-secret", {"verify": False, "timeout": 20})
    client.get_all_orders(symbol='BNBBTC', requests_params={'timeout': 5})

全てのオプションについては、`requests documentation<http://docs.python-requests.org/en/master/>`_を参照してください。

**プロキシ設定**

上記のリクエスト設定を使用することができます。

.. code:: python

    proxies = {
        'http': 'http://10.10.1.10:3128',
        'https': 'http://10.10.1.10:1080'
    }

    # クライアント初期化
    client = Client("api-key", "api-secret", {'proxies': proxies})

    # または、個別のコール
    client.get_all_orders(symbol='BNBBTC', requests_params={'proxies': proxies})

または、リクエスト処理に必要な場合は、プロキシ環境変数を設定することもできます。

Linux 環境変数の設定例（参照：`requests Proxies documentation<http://docs.python-requests.org/en/master/user/advanced/#proxies>`_ ）は下記の通りです。

.. code-block:: bash

    $ export HTTP_PROXY="http://10.10.1.10:3128"
    $ export HTTPS_PROXY="http://10.10.1.10:1080"

Windows環境の場合

.. code-block:: bash

    C:\>set HTTP_PROXY=http://10.10.1.10:3128
    C:\>set HTTPS_PROXY=http://10.10.1.10:1080
