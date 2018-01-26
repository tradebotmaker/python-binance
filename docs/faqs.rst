FAQ　よくある質問
=======

*Q: Why do I get "Timestamp for this request is not valid" なぜ、"Timestamp for this request is not valid"となったのでしょうか？*

*A*: This occurs in 2 different cases.　２つのケースで発生する場合があります。

The timestamp sent is outside of the serverTime - recvWindow value
The timestamp sent is more than 1000ms ahead of the server time
送信したtimestampが、serverTime - recvWindow valueの範囲外の時。
送信したtimestampが、サーバータイムより1000ms以上早い時。

Check that your system time is in sync. See `this issue <https://github.com/sammchardy/python-binance/issues/2#issuecomment-324878152>`_ for some sample code to check the difference between your local
time and the Binance server time.
あなたのシステムの時間が正しいか確認してください。`this issue <https://github.com/sammchardy/python-binance/issues/2#issuecomment-324878152>`_を参照し、ローカルタイムとBinanceサーバータイムの差をチェックするサンプルコードを確認してください。

*Q: Why do I get "Signature for this request is not valid"　なぜ"Signature for this request is not valid"となったのでしょうか？*

*A1*: One of your parameters may not be in the correct format.
パラメータが正しいフォーマットではない可能性があります。

Check recvWindow is an integer and not a string.
recvWindowがstringではなくintegerであることを確認してください。

*A2*: You may need to regenerate your API Key and Secret
API KeyとSecretを再生成する必要があるかもしれません。

*A3*: You may be attempting to access the API from a Chinese IP address, these are now restricted by Binance.
中国のIPアドレスからBinanceへアクセスしようとしている可能性があります。これは現在、Binanceより禁止されています。


*Q: Twisted won't install using pip on Windows WindowsでTwistedをpipを使用してインストールできません。*


*A*:If you see errors building Twisted indication Microsoft Visual C++ is required you may need to install the Visual C++ Build Tools
refer to the `Python Wiki on Widows Compilers <https://wiki.python.org/moin/WindowsCompilers>`_ for your relevant version.
Twistedのビルド中にエラーが発生する場合、Microsoft Visual C++が必要となります。`Python Wiki on Widows Compilers <https://wiki.python.org/moin/WindowsCompilers>`_を参照し、あなたのシステムに対応する正しいバージョンのVisual C++ Build Toolsをインストールしてください。
