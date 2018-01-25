Depth Cache デプスキャッシュ
===========

To follow the depth cache updates for a symbol use the `DepthCacheManager`
通貨ペアのデプスキャッシュの更新をフォローするには、`DepthCacheManager`を使用してください。

Create the manager like so, passing the api client, symbol and an optional callback function.
マネージャーを作成し、api client, symbol（通貨ペア）とオプションのコールバック関数を渡してください。

.. code:: python

    from binance.depthcache import DepthCacheManager
    dcm = DepthCacheManager(client, 'BNBBTC', callback=process_depth)

The callback function receives the current `DepthCache` object which allows access to a pre-sorted
list of bids or asks able to be filtered as required.

Access the symbol value from the `depth_cache` object in case you have multiple caches using the same callback.

By default the depth cache will fetch the order book via REST request every 30 minutes.
This duration can be changed by using the `refresh_interval` parameter. To disable the refresh pass 0 or None.
The socket connection will stay open receiving updates to be replayed once the full order book is received.
コールバック関数は、事前にソートしたbidとaskのリストを含み、フィルタリングも可能な現在の`DepthCache`オブジェクトを受信します。

Websocket Errors Websocketエラー
----------------

If the underlying websocket is disconnected and is unable to reconnect None is returned for the depth_cache parameter.
websocketが切断され、再接続ができない場合、depth_cacheパラメータにはNoneが返されます。

Examples 例
--------

.. code:: python

    # 1 hour interval refresh 1時間ごとの更新
    dcm = DepthCacheManager(client, 'BNBBTC', callback=process_depth, refresh_interval=60*60)

    # disable refreshing 更新を無効にする
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
            # depth cache had an error and needs to be restarted depth cacheにエラーが発生したため、再スタートが必要

At any time the current `DepthCache` object can be retrieved from the `DepthCacheManager`
いつでも現在の`DepthCache`オブジェクトを`DepthCacheManager`から取得することができます。

.. code:: python

    depth_cache = dcm.get_depth_cache()
    if depth_cache is not None:
        print("symbol {}".format(depth_cache.symbol))
        print("top 5 bids")
        print(depth_cache.get_bids()[:5])
        print("top 5 asks")
        print(depth_cache.get_asks()[:5])
    else:
        # depth cache had an error and needs to be restarted　depth cacheにエラーが発生したため、再スタートが必要


To stop the `DepthCacheManager` from returning messages use the `close` method.
This will close the internal websocket and this instance of the `DepthCacheManager` will not be able to be used again.
`DepthCacheManager`のメッセージ送信を停止するには、`close`メソッドを使用してください。
これは、内部websocketを停止し、`DepthCacheManager`は再使用できなくなります。

.. code:: python

    dcm.close()
