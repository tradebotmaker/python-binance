よくある質問
=======

*Q: なぜ、"Timestamp for this request is not valid"となったのでしょうか？*

*A*: ２つのケースで発生する場合があります。

送信したtimestampが、serverTime - recvWindow valueの範囲外の時。
送信したtimestampが、サーバータイムより1000ms以上早い時。

あなたのシステムの時間が正しいか確認してください。 `issue <https://github.com/sammchardy/python-binance/issues/2#issuecomment-324878152>`_ を参照し、ローカルタイムとBinanceサーバータイムの差をチェックするサンプルコードを確認してください。

*Q: なぜ"Signature for this request is not valid"となったのでしょうか？*

*A1*: パラメータが正しいフォーマットではない可能性があります。

recvWindowがstringではなくintegerであることを確認してください。

*A2*: API KeyとSecretを再生成する必要があるかもしれません。

*A3*: 中国のIPアドレスからBinanceへアクセスしようとしている可能性があります。これは現在、Binanceより禁止されています。


*Q: WindowsでTwistedをpipを使用してインストールできません。*


*A*: Twistedのビルド中にエラーが発生する場合、Microsoft Visual C++が必要となります。 `Python Wiki on Widows Compilers <https://wiki.python.org/moin/WindowsCompilers>`_ を参照し、あなたのシステムに対応する正しいバージョンのVisual C++ Build Toolsをインストールしてください。
