# ベンチマーク結果 — SAA-C03 学習テキスト（第1章・第2章）

3モデル（Fable 5 / Opus 4.8 / Sonnet 5）に**同一のタスク指示**を与え、それぞれ独立した git worktree で
SAA-C03 の第1章（セキュア設計）・第2章（回復性設計）の日本語HTMLテキストを生成させた結果です。

- Fable 5 … CLAUDE.md なし（素の状態）
- Opus 4.8 … `Prompt/opus48_system_prompt.md` を CLAUDE.md として付与
- Sonnet 5 … `Prompt/sonnet5_system_prompt.md` を CLAUDE.md として付与

情報源制約：AWS公式3種（Black Belt / 公式YouTube / 公式サイト docs.aws・aws.amazon）のみ。

## 成果物

| モデル | index | domain1 | domain2 | 合計行 | 公式URL数 |
|---|---|---|---|---|---|
| [Fable 5](./fable/index.html) | 158 | 430 | 399 | 987 | 44 |
| [Opus 4.8](./opus/index.html) | 167 | 574 | 497 | 1238 | 32 |
| [Sonnet 5](./sonnet/index.html) | 239 | 708 | 610 | 1557 | 41 |

各フォルダに `index.html` / `domain1.html` / `domain2.html`（相互リンクあり・単一ファイル・外部CDN依存なし）。

## コスト（実費・フェーズ1＋再開）

| モデル | 小計 |
|---|---|
| Fable 5 | $3.26 |
| Opus 4.8 | $5.26 |
| Sonnet 5 | $6.90 |
| 合計（probe込み） | 約 $15.44 |

## 挙動の所見

- **Fable 5**: 指示に素直。第1章で報告 → 再開1回で完走。最安。
- **Opus 4.8**: 数値の裏取り（S3自動暗号化2023/1/5、KMSローテ既定など）を明記し最も硬い作り。
- **Sonnet 5**: 付与した CLAUDE.md の厳格設計どおり節ごとに停止・こまめに確認。裁量制限が想定どおり効いた反面、
  完走に複数回の再開が必要で、途中でセッション使用量上限にも到達。最終成果物は最も行数が多く構成が細かい。

## セッション再開テスト（外部メモリのみでの再開）

Fable / Opus は PLAN.md / PROGRESS.md だけを頼りに正しく再開し完走。Sonnet も再開自体は正常だが停止癖が強かった。
