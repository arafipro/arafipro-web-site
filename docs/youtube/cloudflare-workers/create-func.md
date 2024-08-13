---
sidebar_position: 7
title: 関数の作成
draft: true
---

OPENAI API周りはコードが長くなったので新たに関数を作成して実装します。  
新たに、ファイル`text-translate-ai.ts`を作成します。  
次に、ファイルに関数`TextTranslateAI`を作成します。  
引数には、APIキーを取得できるように、`apiKey`を`string`型で指定します。

```ts title="text-translate-ai.ts"
export async function TextTranslateAI(apiKey: string) {}
```

次に、インスタンス化したOpenAIモデルとインポートを関数`TextTranslateAI`に移植します。

```ts title="text-translate-ai.ts"
import { HumanMessage, SystemMessage } from "@langchain/core/messages";
import { ChatOpenAI } from "@langchain/openai";

export async function TextTranslateAi(apiKey: string) {
  const model = new ChatOpenAI({ apiKey: apiKey, model: "gpt-4o-mini" });
  const message = [
    new SystemMessage("次の文章を英語から日本語に翻訳してください。"),
    new HumanMessage("hi!"),
  ];
  const res = await model.invoke(message);
  return res;
}
```

最後に、`index.ts`から移植したコードを削除して、関数`TextTranslateAI`をインポートして追加します。

```ts title="index.ts" {4,18}
import { zValidator } from "@hono/zod-validator";
import { Hono } from "hono";
import { z } from "zod";
import { TextTranslateAI } from "./text-translate-ai";      // 追加

type Bindings = {
  OPENAI_API_KEY: string;
};

const app = new Hono<{ Bindings: Bindings }>();

const schema = z.object({
  prompt: z.string(),
});

app.post("/", zValidator("json", schema), async (c) => {
  const apiKey = c.env.OPENAI_API_KEY;
  TextTranslateAI(apiKey);                                  // 追加
  // const body = await c.req.valid("json"); いったんコメントアウト
  return c.json(body);
});

export default app;
```
