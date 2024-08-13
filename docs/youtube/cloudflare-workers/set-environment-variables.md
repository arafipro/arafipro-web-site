---
sidebar_position: 5
title: 環境変数を設定
draft: true
---

```ts title="index.ts"
import { zValidator } from "@hono/zod-validator";
import { ChatOpenAI } from "@langchain/openai";
import { Hono } from "hono";
import { z } from "zod";

type Bindings = {
  OPENAI_API_KEY: string;
};

type Variables = {
  openai: ChatOpenAI;
};

const app = new Hono<{ Bindings: Bindings; Variables: Variables }>();

const schema = z.object({
  prompt: z.string(),
});

app.post("/", zValidator("json", schema), async (c) => {
  const apiKey = c.env.OPENAI_API_KEY;
  const model = new ChatOpenAI({ model: "gpt-4o-mini", apiKey: apiKey });
  const body = await c.req.valid("json");
  return c.json(body);
});

export default app;
```

```ts title="text-translate-ai.ts"
import { HumanMessage, SystemMessage } from "@langchain/core/messages";
import { StringOutputParser } from "@langchain/core/output_parsers";
import { ChatOpenAI } from "@langchain/openai";

export async function TextTranslateAI(api_key: string) {
  const model = new ChatOpenAI({ model: "gpt-4o-mini", apiKey: api_key });
  const messages = [
    new SystemMessage("Translate the following from English into Italian"),
    new HumanMessage("hi!"),
  ];
  const parser = new StringOutputParser();
  const result = await model.invoke(messages);
  return await parser.invoke(result);
}
```
