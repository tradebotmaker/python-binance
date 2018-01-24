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

At the current time Binance rate limits are:

- 1200 requests per minute
- 10 orders per second
- 100,000 orders per 24hrs

Some calls have a higher weight than others especially if a call returns information about all symbols.
Read the `official Binance documentation <https://github.com/binance-exchange/binance-official-api-docs`_ for specific information.

.. image:: https://analytics-pixel.appspot.com/UA-111417213-1/github/python-binance/docs/overview?pixel

Requests Settings
-----------------

`python-binance` uses the `requests <http://docs.python-requests.org/en/master/>`_ library.

You can set custom requests parameters for all API calls when creating the client.

.. code:: python

    client = Client("api-key", "api-secret", {"verify": False, "timeout": 20})

You may also pass custom requests parameters through any API call to override default settings or the above settingsspecify new ones like the example below.

.. code:: python

    # this would result in verify: False and timeout: 5 for the get_all_orders call
    client = Client("api-key", "api-secret", {"verify": False, "timeout": 20})
    client.get_all_orders(symbol='BNBBTC', requests_params={'timeout': 5})

Check out the `requests documentation <http://docs.python-requests.org/en/master/>`_ for all options.

**Proxy Settings**

You can use the Requests Settings method above

.. code:: python

    proxies = {
        'http': 'http://10.10.1.10:3128',
        'https': 'http://10.10.1.10:1080'
    }

    # in the Client instantiation
    client = Client("api-key", "api-secret", {'proxies': proxies})

    # or on an individual call
    client.get_all_orders(symbol='BNBBTC', requests_params={'proxies': proxies})

Or set an environment variable for your proxy if required to work across all requests.

An example for Linux environments from the `requests Proxies documentation <http://docs.python-requests.org/en/master/user/advanced/#proxies>`_ is as follows.

.. code-block:: bash

    $ export HTTP_PROXY="http://10.10.1.10:3128"
    $ export HTTPS_PROXY="http://10.10.1.10:1080"

For Windows environments

.. code-block:: bash

    C:\>set HTTP_PROXY=http://10.10.1.10:3128
    C:\>set HTTPS_PROXY=http://10.10.1.10:1080
