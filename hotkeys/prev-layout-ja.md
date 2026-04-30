---
title: prefix+p — 前のレイアウト
---

# prefix+p — 前のレイアウトに切り替え

> **言語** · [中文](prev-layout-zh.md) · [English](prev-layout-en.md) · **日本語**

[← 設定ヘルプに戻る](../settings-help-ja.md)

`prefix+p` は [`prefix+n`](next-layout-ja.md) の逆方向: マッチするレイアウトリストで前に 1 つ進みます。

> デフォルト chord: `p` · 設定項目: `prev_layout` · 「設定 → ホットキー → レイアウトナビゲーション」で変更

---

## 動作

- リストの範囲、順序、スキップルールはすべて `prefix+n` と完全に同じ。
- 先頭から末尾に戻る（逆順サイクル）。
- 連続押しが可能。
- サイレント拒否条件も同じ。

---

## next_layout との関係

`prefix+n` + `prefix+p` をペアで使う: `n` で進みすぎたら `p` で 1 つ戻す。

2 つのレイアウト間だけを行き来する場合は、`n/p` よりも [`prefix+l`](last-layout-ja.md) のほうが手軽です —— 何回進んだかを数える必要がありません。

---

詳細は [prefix+n ドキュメント](next-layout-ja.md) を参照。
