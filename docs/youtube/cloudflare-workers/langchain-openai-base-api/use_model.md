---
sidebar_position: 6
title: OpenAIモデルの使用
draft: true
---

インスタンス化したOpenAIモデル`model`を使用して、`invoke`メソッドにメッセージのリストを渡します。
たとえば、以下のようにして、モデルにメッセージのリストを渡し、その応答を受け取ることができます。

まずは、`@langchain/core/messages`から`HumanMessage`と`SystemMessage`をインポートします。

```ts
import { HumanMessage, SystemMessage } from "@langchain/core/messages";
```

次に、`messages`という配列を用意します。  
配列には、`SystemMessage`ではシステムのメッセージを定義します。  
また、`HumanMessage`ではユーザーのメッセージを定義します。  
そして、`model`から`invoke`メソッドを呼び出して、`messages`を渡します。
そうすることで、モデルに対して翻訳の指示と翻訳対象のテキストを送信しています。  
この`invoke`メソッドの戻り値は、`AIMessage`オブジェクトとなります。
これには、文字列の応答と応答に関するその他のメタデータが含まれます。

```ts
const messages = [
  new SystemMessage("次の文章を英語から日本語に翻訳してください。"),
  new HumanMessage("hi!"),
];
const res = await model.invoke(messages);
// const body = await c.req.valid("json");
return c.json(res);
```

ただ、多くの場合、文字列の応答のみを使用したい場合があります。
その場合は、単純な出力パーサーを使用して、文字列の応答のみを抽出できます。
