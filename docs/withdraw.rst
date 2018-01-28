出金エンドポイント
==================

`出金処理 <binance.html#binance.client.Client.withdraw>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

この呼び出しをするには、APIキーに対してWithdrawal permissions（出金許可）を許可する必要があります。

APIを使用して引き出すには、以前にウェブサイトから出金したことがあり、その登録Eメールを使用してAPIで引き出す許可を与える必要があります。

出金に失敗すると、 `BinanceWithdrawException <binance.html#binance.exceptions.BinanceWithdrawException>`_ が発生します。

.. code:: python

    from binance.exceptions import BinanceAPIException, BinanceWithdrawException
    try:
        # ネームパラメータを渡していない場合は、クライアントからアセットバリューに渡される
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

    # ネームパラメータを渡す
    result = client.withdraw(
        asset='ETH',
        address='<eth_address>',
        amount=100,
        name='Withdraw')

    # XRPやXMRのように、コインが追加のタグまたは名前を必要とする場合、`addressTag`を渡す
    result = client.withdraw(
        asset='XRP',
        address='<xrp_address>',
        addressTag='<xrp_address_tag>',
        amount=10000)

`デポジットの履歴を取得 <binance.html#binance.client.Client.get_deposit_history>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    deposits = client.get_deposit_history()
    btc_deposits = client.get_deposit_history(asset='BTC')


`出金の履歴を渡す <binance.html#binance.client.Client.get_withdraw_history>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    withdraws = client.get_withdraw_history()
    btc_withdraws = client.get_withdraw_history(asset='BTC')

`デポジットアドレスを取得 <binance.html#binance.client.Client.get_deposit_address>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    address = client.get_deposit_address(asset='BTC')
