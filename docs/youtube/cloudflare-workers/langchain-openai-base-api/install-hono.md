---
sidebar_position: 3
title: Honoを導入
draft: true
---

## Honoをインストール

Honoをインストールするには、以下のコマンドを使用します。

```sh
bun add hono
```

## 動作確認

次に簡単なコードを作成します。  
POSTメソッドで、json形式のデータを受け取ります。  
そして、そのまま出力するようにします。

```ts title="src/index.ts"
import { Hono } from "hono";

const app = new Hono();

app.post("/", async (c) => {
  const body = await c.req.json();
  return c.json(body);
});

export default app;
```

動作しましたが、このままではどんなJSONも受け取るようになっています。  
これを、決まったJSONを受け取るようにします。  
それには、zodを使用して、バリデーションスキーマを作成します。

03
ターミナルを開いて、簡易サーバーを立ち上げます。
サンダークライアントを開いて、動作を確認します。
new requestから、新たに開いて、URLのところに、ターミナルに表示されているURLをコピペします。
getをpostに変更して、bodyタグからJSONを選択して、適当にJSONを入力します。
実行すると、そのまま出力されます。
入力したJSONを変更して、実行すると、そのまま出力されます。
ただ、このままではどんなJSONも受け取るようになっています。
これを、決まったJSONを受け取るようにします。
それには、zodを使用して、バリデーションスキーマを作成します。

04
ターミナルを新たに開きます。

05
## zodを使用したバリデーション

Honoには、zodを利用するためのミドルウェアが用意されています。  
以下のコマンドを使用します。

```sh
bun add @hono/zod-validator
```

06

```ts title="src/index.ts" {1,3,7-9,11,12}
import { zValidator } from "@hono/zod-validator";
import { Hono } from "hono";
import { z } from "zod";

const app = new Hono();

const schema = z.object({
  prompt: z.string(),
});
// 07
app.post("/", zValidator("json", schema), async (c) => { // 変更
  // 08
  const body = await c.req.valid("json");                // 変更
  return c.json(body);
});

export default app;
```

09
サンダークライアントを開いて、動作を確認します。
実行すると、今度はエラーが発生します。
そこで、スキーマで設定したpromptに変更します。
もう1度実行すると、次は表示されました。
バリデーションが効いています。

これで、スキーマで設定したバリデーションが効いています。
また、型`Prompt`を定義して、受け取るjson形式を指定します。
