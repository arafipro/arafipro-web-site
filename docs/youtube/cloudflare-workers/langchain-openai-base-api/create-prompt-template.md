---
sidebar_position: 8
title: PromptTemplateの作成
draft: true
---

PromptTemplateは、生のユーザー入力を言語モデルに渡す準備ができたデータ（プロンプト）に変換するのに役立つように設計されたLangChainの概念です。  
PromptTemplateは、生のユーザー入力を受け取り、言語モデルに渡す準備ができたデータ（プロンプト）を返します。  
一般的な変換には、システムメッセージの追加や、ユーザー入力を使用したテンプレートのフォーマットなどがあります。

## PromptTemplateの作成

この例では、テキストを別の言語に翻訳するPromptTemplateを作成します。  
このPromptTemplateは、次の2つのユーザー変数を受け取ります。

- language：テキストの翻訳先言語
- text：翻訳するテキスト

まず、システムメッセージとしてフォーマットする文字列を作成します。

```ts
const systemTemplate = "次の文章を英語から{language}に翻訳してください。";
```

次に、`systemTemplate`とテキストの配置場所を指定するより単純なテンプレートを組み合わせて、PromptTemplateを作成します。
まずは、`@langchain/core/prompts`から`ChatPromptTemplate`をインポートします。

```ts
import { ChatPromptTemplate } from "@langchain/core/prompts";
```

次に、定数`promptTemplate`を用意します。
`ChatPromptTemplate`から`fromMessages`メソッドを呼び出します。
`fromMessages`メソッドの引数には、配列の中に2つの配列を用意します。
1つ目には、キーを`system`で、値を`systemTemplate`とします。
2つ目には、キーを`user`で、値を`{text}`とします。

```ts
const promptTemplate = ChatPromptTemplate.fromMessages([
  ["system", systemTemplate],
  ["user", "{text}"],
]);
```

## PromptTemplateの使用

定数`promptTemplate`から、`invoke`メソッドを呼び出します。
`invoke`メソッドの引数には、`language`プロパティと`text`プロパティのオブジェクトを渡します。  
`promptTemplate`単独でどのような動作をするのかを確認するために、以下のように試すことができます。

```ts
const res = await promptTemplate.invoke({ language: "japanese", text: "hi" });
```
