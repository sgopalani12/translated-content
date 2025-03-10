---
title: String.prototype.replaceAll()
slug: Web/JavaScript/Reference/Global_Objects/String/replaceAll
tags:
  - JavaScript
  - Method
  - Prototype
  - Reference
  - String
  - regex
translation_of: Web/JavaScript/Reference/Global_Objects/String/replaceAll
---
{{JSRef}}

**`replaceAll()`** メソッドは、`pattern` にマッチしたすべての文字列を `replacement` で置き換えた新しい文字列を返します。`pattern` は文字列または {{jsxref("RegExp")}} を指定することができ、`replacement` は文字列または各マッチに対して呼び出される関数を指定することができます。

元の文字列は変更されません。

{{EmbedInteractiveExample("pages/js/string-replaceall.html")}}

## 構文

```
const newStr = str.replaceAll(regexp|substr, newSubstr|function)
```

> **Note:** \`_regexp_\`を使用する場合は、グローバル("g")フラグを設定する必要があります。それ以外の場合は、`TypeError` が投げられます："replaceAll must be called with a global RegExp".

### 引数

- `regexp` (pattern)
  - : グローバルフラグを持つ {{jsxref("RegExp")}} オブジェクトまたはリテラルです。マッチしたものは `newSubstr` または、指定された `function` によって返された値に置き換えられます。グローバル("g")フラグのない RegExp は `TypeError` を投げます："replaceAll must be called with a global RegExp".
- `substr`
  - : `newSubstr` で置き換えられる {{jsxref("String")}} です。これは文字列リテラルとして扱われ、正規表現として解釈されません。
- `newSubstr` (replacement)
  - : `regexp` または `substr` で指定された部分文字列を置き換える {{jsxref("String")}} です。いくつかの特殊な置換パターンがサポートされています。下記の「[引数としての文字列の指定](#Specifying_a_string_as_a_parameter)」セクションで説明しています。
- `function` (replacement)
  - : 指定された `regexp` または `substr` のマッチを置き換えるために使用される、新しい部分文字列を生成するために呼び出される関数です。この関数に与えられる引数については、下記の「[引数としての関数の指定](#Specifying_a_function_as_a_parameter)」セクションで説明しています。

### 戻り値

パターンにマッチしたすべての文字列を置換文字列で置き換えた新しい文字列です。

## 説明

このメソッドは、呼び出し元の {{jsxref("String")}} オブジェクトを変更しません。戻り値として新しい文字列を返します。

### 引数としての文字列の指定

置換文字列には以下の特殊な置換パターンを含めることができます。

| パターン                                | 挿入                                                                                                                                                                                                           |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$$`​ ​ ​​ ​​ ​​ ​​ ​​ ​​ ​​ ​​ ​​ ​​ ​ | `"$"` を挿入します。                                                                                                                                                                                           |
| `$&`                                    | マッチした部分文字列を挿入します。                                                                                                                                                                             |
| `` $` ``                                | マッチした部分文字列の直前の文字列の部分を挿入します。                                                                                                                                                         |
| `$'`                                    | マッチした部分文字列の直後の文字列の部分を挿入します。                                                                                                                                                         |
| `$n`                                    | `n` は 100 未満の正の整数です。第一引数が {{jsxref("RegExp")}} オブジェクトだった場合に `n` 番目の括弧でキャプチャされた文字列を挿入します。`1`, `2`, ... でインデックスされることに注意してください。 |

### 引数としての関数の指定

第二引数として関数を指定することができます。このとき、関数はマッチが完了した後に実行されます。関数呼び出しの結果（返り値）は、置換文字列として使われます（**注:** 上記の特殊な置換パターンはこの場合には適用されません）。

第一引数の正規表現がグローバルだと、置換されるべきマッチごとに関数が複数回実行されうることに注意してください。

関数に与えられる引数は次の通りです。

| 名前                                          | 与えられる値                                                                                                                                                                                                                                                                        |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `match` ​ ​​ ​​ ​​ ​​ ​​ ​​ ​​ ​​ ​​ ​​ ​ ​ ​ | マッチした部分文字列（上記の `$&` に対応）です。                                                                                                                                                                                                                                    |
| `p1, p2, ...`                                 | `replace()` の第一引数が {{jsxref("RegExp")}} オブジェクトだった場合、_n_ 番目の括弧でキャプチャされたグループの文字列（上記の `$1`, `$2`, などに対応）です。例えば `/(\a+)(\b+)/` が与えられた場合、`p1` は `\a+` に対するマッチ、`p2` は `\b+` に対するマッチとなります。 |
| `offset`                                      | マッチした部分文字列の、分析中の文字列全体の中でのオフセットです（例えば、文字列全体が `'abcd'` で、マッチした部分文字列が `'bc'` ならば、この引数は `1` となります）。                                                                                                             |
| `string`                                      | 分析中の文字列全体です。                                                                                                                                                                                                                                                            |

（引数の正確な個数は、第一引数が {{jsxref("RegExp")}} オブジェクトかどうか、そうならばさらに括弧でキャプチャされるサブマッチがいくつ指定されているかに依ります。）

## 例

### replaceAll の使用

```js
'aabbcc'.replaceAll('b', '.');
// 'aa..cc'
```

### グローバルではない正規表現

正規表現フラグを使用する場合は、グローバルである必要があります。これは機能しません：

```js example-bad
'aabbcc'.replaceAll(/b/, '.');
TypeError: replaceAll must be called with a global RegExp
```

これは機能します：

```js example-good
'aabbcc'.replaceAll(/b/g, '.');
"aa..cc"
```

## 仕様

| 仕様書                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------ |
| {{SpecName('ESDraft', '#sec-string.prototype.replaceall', 'String.prototype.replaceAll')}} |

## ブラウザー実装状況

{{Compat("javascript.builtins.String.replaceAll")}}

## 関連情報

- {{jsxref("String.prototype.replace", "String.prototype.replace()")}}
- {{jsxref("String.prototype.match", "String.prototype.match()")}}
- {{jsxref("RegExp.prototype.exec", "RegExp.prototype.exec()")}}
- {{jsxref("RegExp.prototype.test", "RegExp.prototype.test()")}}
