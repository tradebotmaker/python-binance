Getting Started はじめに
========================

Installation　インストール
-------------------------

``python-binance`` is available on `PYPI <https://pypi.python.org/pypi/python-binance/>`_.
Install with ``pip``:
``python-binance``は``pip``により`PYPI <https://pypi.python.org/pypi/python-binance/>`_からインストールできます。

.. code:: bash

    pip install python-binance

**Windows**

If you see errors building Twisted indication Microsoft Visual C++ is required you may need to install the Visual C++ Build Tools
refer to the `Python Wiki on Widows Compilers <https://wiki.python.org/moin/WindowsCompilers>`_ for your relevant version.
ビルドエラーが発生する場合、Microsoft Visual C++が必要です。`Python Wiki on Widows Compilers <https://wiki.python.org/moin/WindowsCompilers>`_を参照して、Visual C++ Build Toolsをインストールしてください。

Register on Binance　Binanceでの登録
----------------------------------------------

Firstly `register an account with Binance <https://www.binance.com/register.html?ref=10099792>`_.

まず最初に、`Binanceで登録 <https://www.binance.com/register.html?ref=10099792>`_ してください。

Generate an API Key API Keyの取得
---------------------------------

To use signed account methods you are required to `create an API Key  <https://www.binance.com/userCenter/createApi.html>`_.
登録した口座でプログラムを使用するには、`API Keyを作成  <https://www.binance.com/userCenter/createApi.html>`_する必要があります。

Initialise the client　クライアントの初期化
-------------------------------------------

Pass your API Key and Secret
API KeyとSecretを設定します。

.. code:: python

    from binance.client import Client
    client = Client(api_key, api_secret)

Making API Calls　APIの呼び出し
------------------------------

Every method supports the passing of arbitrary parameters via keyword matching those in the`Binance API documentation <https://github.com/binance-exchange/binance-official-api-docs>`_.
These keyword arguments will be sent directly to the relevant endpoint.
`Binance API documentation <https://github.com/binance-exchange/binance-official-api-docs>`_で設定されているキーワードと同じ単語を使用した各メソッドは、任意のパラメータを対応したエンドポイントに渡します。

Each API method returns a dictionary of the JSON response as per the `Binance API documentation <https://github.com/binance-exchange/binance-official-api-docs>`_.
The docstring of each method in the code references the endpoint it implements.
`Binance API documentation <https://github.com/binance-exchange/binance-official-api-docs>`_に記載されている通り、各メソッドは、JSONレスポンスのディクショナリーを返します。
各メソッドのコードのdocstringは、元となったエンドポイントの内容を参照しています。

The Binance API documentation references a `timestamp` parameter, this is generated for you where required.
Binance API documentationは、`timestamp`パラメータを参照しますが、要求された場合は生成されます。

Some methods have a `recvWindow` parameter for `timing security, see Binance documentation <https://github.com/binance-exchange/binance-official-api-docs/blob/master/rest-api.md#timing-security>`_.
メソッドによっては`recvWindow`パラメータがあります。`タイミングセキュリティーのために使用されます。詳細はBinance documentationを参照してください。 <https://github.com/binance-exchange/binance-official-api-docs/blob/master/rest-api.md#timing-security>`_

API Endpoints are rate limited by Binance at 20 requests per second, ask them if you require more.
APIエンドポイントのレートリミットは、Binanceにより秒間20リクエストに制限されています。制限を解除したい場合は、直接お問い合わせください。

API Rate Limit
--------------

Check the `get_exchange_info() <binance.html#binance.client.Client.get_exchange_info>`_ call for up to date rate limits.
`get_exchange_info() <binance.html#binance.client.Client.get_exchange_info>`_を確認して、最新のレートリミットに関する情報を入手してください。

At the current time Binance rate limits are:
現時点でのBinanceのレートリミット：

- 1200 requests per minute
- 10 orders per second
- 100,000 orders per 24hrs

- １分間に1200リクエスト
- １秒間に10の注文
- 24時間に100,000の注文

Some calls have a higher weight than others especially if a call returns information about all symbols.
Read the `official Binance documentation <https://github.com/binance-exchange/binance-official-api-docs`_ for specific information.
呼び出すメソッドによっては、全ての通貨ペアの情報を読み込む場合など、他のメソッドよりも負荷がかかる場合があります。
詳細は、`official Binance documentation <https://github.com/binance-exchange/binance-official-api-docs`_ をご確認ください。

.. image:: https://analytics-pixel.appspot.com/UA-111417213-1/github/python-binance/docs/overview?pixel

Requests Settings Requestの設定
--------------------------------

`python-binance` uses the `requests <http://docs.python-requests.org/en/master/>`_ library.
`python-binance` は、 `requests <http://docs.python-requests.org/en/master/>`_ ライブラリを使用します。

You can set custom requests parameters for all API calls when creating the client.
クライアントを作成後、全てのAPIコールに対し、カスタムリクエストパラメータを設定できます。

.. code:: python

    client = Client("api-key", "api-secret", {"verify": False, "timeout": 20})

You may also pass custom requests parameters through any API call to override default settings or the above settingsspecify new ones like the example below.
どのAPIコールでも、デフォルト設定をオーバーライドまたは再設定することにより、カスタムリクエストパラメータを送信することができます。

.. code:: python

    # this would result in verify: False and timeout: 5 for the get_all_orders call
    client = Client("api-key", "api-secret", {"verify": False, "timeout": 20})
    client.get_all_orders(symbol='BNBBTC', requests_params={'timeout': 5})

Check out the `requests documentation <http://docs.python-requests.org/en/master/>`_ for all options.
全てのオプションについては、`requests documentation <http://docs.python-requests.org/en/master/>`_を参照してください。

**Proxy Settings プロキシ設定**

You can use the Requests Settings method above
上記のリクエスト設定を使用することができます。

.. code:: python

    proxies = {
        'http': 'http://10.10.1.10:3128',
        'https': 'http://10.10.1.10:1080'
    }

    # in the Client instantiation　クライアント初期化
    client = Client("api-key", "api-secret", {'proxies': proxies})

    # or on an individual call または、個別のコール
    client.get_all_orders(symbol='BNBBTC', requests_params={'proxies': proxies})

Or set an environment variable for your proxy if required to work across all requests.
または、リクエスト処理に必要な場合は、プロキシ環境変数を設定することもできます。

An example for Linux environments from the `requests Proxies documentation <http://docs.python-requests.org/en/master/user/advanced/#proxies>`_ is as follows.
Linux 環境変数の設定例（参照：`requests Proxies documentation <http://docs.python-requests.org/en/master/user/advanced/#proxies>`_ ）は下記の通りです。

.. code-block:: bash

    $ export HTTP_PROXY="http://10.10.1.10:3128"
    $ export HTTPS_PROXY="http://10.10.1.10:1080"

For Windows environments Windows環境の場合

.. code-block:: bash

    C:\>set HTTP_PROXY=http://10.10.1.10:3128
    C:\>set HTTPS_PROXY=http://10.10.1.10:1080
