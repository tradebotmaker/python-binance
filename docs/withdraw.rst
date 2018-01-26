Withdraw Endpoints 引き出しエンドポイント
==================

`Place a withdrawal 引き出し処理<binance.html#binance.client.Client.withdraw>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Make sure you enable Withdrawal permissions for your API Key to use this call.
この呼び出しをするには、APIキーに対してWithdrawal permissions（引き出し許可）を許可する必要があります。

You must have withdrawn to the address through the website and approved the withdrawal via email before you can withdraw using the API.
APIを使用して引き出すには、以前にウェブサイトから引き出したことがあり、その登録Eメールを使用してAPIで引き出す許可を与える必要があります。

Raises a `BinanceWithdrawException <binance.html#binance.exceptions.BinanceWithdrawException>`_ if the withdraw fails.
引き出しに失敗すると、`BinanceWithdrawException <binance.html#binance.exceptions.BinanceWithdrawException>`_が発生します。

.. code:: python

    from binance.exceptions import BinanceAPIException, BinanceWithdrawException
    try:
        # name parameter will be set to the asset value by the client if not passed ネームパラメータを渡していない場合は、クライアントからアセットバリューに渡される
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

    # passing a name parameter ネームパラメータを渡す
    result = client.withdraw(
        asset='ETH',
        address='<eth_address>',
        amount=100,
        name='Withdraw')

    # if the coin requires a extra tag or name such as XRP or XMR then pass an `addressTag` parameter. XRPやXMRのように、コインが追加のタグまたは名前を必要とする場合、`addressTag`を渡す
    result = client.withdraw(
        asset='XRP',
        address='<xrp_address>',
        addressTag='<xrp_address_tag>',
        amount=10000)

`Fetch deposit history デポジットの履歴を取得<binance.html#binance.client.Client.get_deposit_history>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    deposits = client.get_deposit_history()
    btc_deposits = client.get_deposit_history(asset='BTC')


`Fetch withdraw history 出金の履歴を渡す<binance.html#binance.client.Client.get_withdraw_history>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    withdraws = client.get_withdraw_history()
    btc_withdraws = client.get_withdraw_history(asset='BTC')

`Get deposit address デポジットアドレスを取得<binance.html#binance.client.Client.get_deposit_address>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    address = client.get_deposit_address(asset='BTC')
