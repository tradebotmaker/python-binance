例外
==========

BinanceResponseException
------------------------

JSONレスポンス以外が返された時に発生

BinanceAPIException
-------------------

APIコールエラーの際に、binance.exceptions.BinanceAPIExceptionが発生します。

例外時は、下記の項目にアクセスできます。

- `status_code` - レスポンスステータスコード
- `response` - レスポンスオブジェクト
- `code` - Binanceエラーコード
- `message` - Binanceエラーメッセージ
- `request` - ある場合はリクエストオブジェクト

.. code:: python

    try:
        client.get_all_orders()
    except BinanceAPIException as e:
        print e.status_code
        print e.message

BinanceOrderException
---------------------

注文を送信する際、 `Binance Trading Rules <https://binance.zendesk.com/hc/en-us/articles/115000594711>`_ にパラメータが適合するか確認するため、バリデートされます。

下記の例外は、 `BinanceOrderException` を継承します。

BinanceOrderMinAmountException
------------------------------

指定されたamountがtrade minimum amountの倍数ではない時に発生。

BinanceOrderMinPriceException
-----------------------------

priceがtrade minimum priceよりも小さい時に発生。

BinanceOrderTotalPriceException
-------------------------------

totalがtrade minimum totalよりも小さい時に発生。

BinanceOrderUnknownSymbolException
----------------------------------

適合するsymbolが存在しない時に発生。

BinanceOrderInactiveSymbolException
-----------------------------------

symbolがアクティブではない時に発生。

BinanceWithdrawException
------------------------

出金に失敗した時に発生。
