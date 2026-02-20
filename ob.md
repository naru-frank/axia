# Claude 設定ファイル

> **ナルさんへ**：作業を依頼する際に「設定ファイルを読んで、○○の指示に従って〜して」と伝えると、Claudeが正しい手順で動きます。

---

## このファイルの扱い方

- **場所**: `~/claude_config.md`（ホームフォルダ直下）
- **編集**: テキストエディタで直接編集してよい
- **参照指示の例**:
  - 「設定ファイルを読んで、ObsidianのセクションのルールでVaultを更新して」
  - 「設定ファイルを読んで、VPS操作の手順でサーバーを確認して」
- **セクションの追加方法**: `## 新しい作業カテゴリ` を追記するだけでよい
- **Claude側のルール**: このファイルを読んだら、該当セクションの指示を必ず守って作業すること

---

## Obsidian 連携

### 作業開始前に必ずやること

1. `_Claude/技術メモ_ObsidianPatch仕様.md` を `mcp-obsidian:obsidian_get_file_contents` で読む
2. 仕様を確認してから操作に入る

### 使うツール

- **patch操作**: `mcp-obsidian:obsidian_patch_content` のみ使う
- `obsidian-mcp-tools:patch_active_file` / `patch_vault_file` は heading 操作が動かないので使わない

### 見出し指定のルール（最重要）

**パスは必ず H1（ドキュメントタイトル）から始める。**
```
# 正しい例
target: "H1タイトル::H2セクション::H3サブセクション"

# NG例（invalid-target になる）
target: "H3サブセクション"              # 子見出し単体
target: "H2セクション::H3サブセクション" # H2からのパス
```

操作前に `obsidian_get_file_contents` でファイル冒頭を確認し、H1タイトルを必ず把握してからパスを組み立てる。

### エラー対処

| エラー | 対処 |
|--------|------|
| `invalid-target` | H1からのフルパスに修正 |
| `content-already-preexists-in-target` | `append` を使うか全体書き換え |

### 全体書き換えのフォールバック

patch がどうしても通らない場合のみ：
1. `obsidian-mcp-tools:show_file_in_obsidian` でアクティブに設定
2. `obsidian-mcp-tools:get_active_file` で全文取得
3. `obsidian-mcp-tools:update_active_file` で全体書き換え
