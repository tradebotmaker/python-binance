Depth Cache デプスキャッシュ
===========

通貨ペアのデプスキャッシュの更新をフォローするには、 `DepthCacheManager` を使用してください。

マネージャーを作成し、api client, symbol（通貨ペア）とオプションのコールバック関数を渡してください。

.. code:: python

    from binance.depthcache import DepthCacheManager
    dcm = DepthCacheManager(client, 'BNBBTC', callback=process_depth)

コールバック関数は、事前にソートしたbidとaskのリストを含み、フィルタリングも可能な現在の `DepthCache` オブジェクトを受信します。

同じコールバックを使用する複数のキャッシュがある場合は、 `depth_cache` オブジェクトからsymbolの値にアクセスしてください。

デフォルトでは、デプスキャッシュはRESTリクエストを使用して30分ごとにオーダーブックを取得します。
この間隔は、 `refresh_interval` で変更することができます。停止させる場合は、0またはNoneを設定してください。
ソケット接続は、フルオーダーブックの受信以降、更新を受信するため常にオープンとなります。

Websocketエラー
----------------

websocketが切断され、再接続ができない場合、depth_cacheパラメータにはNoneが返されます。

例
--------

.. code:: python

    # 1時間ごとの更新
    dcm = DepthCacheManager(client, 'BNBBTC', callback=process_depth, refresh_interval=60*60)

    # 更新を無効にする
    dcm = DepthCacheManager(client, 'BNBBTC', callback=process_depth, refresh_interval=0)

.. code:: python

    def process_depth(depth_cache):
        if depth_cache is not None:
            print("symbol {}".format(depth_cache.symbol))
            print("top 5 bids")
            print(depth_cache.get_bids()[:5])
            print("top 5 asks")
            print(depth_cache.get_asks()[:5])
        else:
            # depth cacheにエラーが発生したため、再スタートが必要

いつでも現在の `DepthCache` オブジェクトを `DepthCacheManager` から取得することができます。

.. code:: python

    depth_cache = dcm.get_depth_cache()
    if depth_cache is not None:
        print("symbol {}".format(depth_cache.symbol))
        print("top 5 bids")
        print(depth_cache.get_bids()[:5])
        print("top 5 asks")
        print(depth_cache.get_asks()[:5])
    else:
        # depth cacheにエラーが発生したため、再スタートが必要


`DepthCacheManager` のメッセージ送信を停止するには、 `close` メソッドを使用してください。
これは、内部websocketを停止し、 `DepthCacheManager` は再使用できなくなります。

.. code:: python

    dcm.close()
