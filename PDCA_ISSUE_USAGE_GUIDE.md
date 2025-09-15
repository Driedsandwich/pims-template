# PIMS（PDCA-Issue Muramasa System）運用ガイド

## 🎯 PIMSとは

PIMSは、GitHub IssuesとPDCAサイクルを組み合わせた継続的改善システムです。各Issueを1つのPDCAサイクルとして運用し、知識を蓄積しながらプロジェクトを進化させます。

## 🚀 導入手順

### 1. 基本セットアップ

```bash
# テンプレートから新規作成
gh repo create <owner>/<repo> --public --template Driedsandwich/pims-template

# 既存リポジトリに導入
gh repo clone Driedsandwich/pims-template
cp -r .github <your-repo>/
cp PDCA_ISSUE_USAGE_GUIDE.md <your-repo>/
```

### 2. ラベルセットアップ

```bash
# GitHub CLIでワークフロー実行
gh workflow run pims-setup.yml -f setup_labels=true

# 手動でラベル作成
gh api repos/<owner>/<repo>/labels -f name="pdca-phase:plan" -f color="0052CC" -f description="PDCA Phase: Plan"
```

### 3. knowledge.mdc初期化

```bash
# .cursor/rules/ディレクトリ作成
mkdir -p .cursor/rules

# knowledge.mdc初期テンプレート作成
cat > .cursor/rules/knowledge.mdc << 'EOF'
# PIMS知識蓄積ファイル

## 更新履歴
- 2025-09-15: 初期化

## 成功パターン

## 失敗パターン

## 再発防止ルール
EOF
```

## 📋 Issue運用フロー

### PDCA開発サイクル

1. **Plan（計画）**
   - 目的・成功基準を明確化
   - 前提・制約・依存関係を整理
   - ラベル: `pdca-phase:plan`

2. **Do（実行）**
   - 実装・テスト・ドキュメント作成
   - 手順・ログ・成果物を記録
   - ラベル: `pdca-phase:do`

3. **Check（検証）**
   - 結果・副作用・再現性を確認
   - 成功基準との照合
   - ラベル: `pdca-phase:check`

4. **Act（改善）**
   - 是正・ルール反映・派生Issue作成
   - PIMS気づきを記録
   - ラベル: `pdca-phase:act`

### 緊急修復Issue

- 症状・影響・再現手順を詳細記録
- 原因仮説・修復手順・代替案を明記
- ラベル: `emergency`, `fix`, `pims`

## 🔄 Pull Request運用

### 必須セクション

- **Decision（判断）**: 選択肢・採否理由・影響
- **Evidence（証跡）**: build/type-check/実行ログ/スクショ/URL
- **PIMS気づき**: 気づき・根拠・反映先
- **担当**: サービス/モデル/端末/証跡URL

## 👤 担当者記録原則

### AIサービス/モデル/端末の記録

```
- サービス: Cursor / ClaudeCode / CodexCLI / GitHub Copilot
- モデル: GPT-4o / GPT-5 / Sonnet-4 / Claude-3.5
- 端末: MainPC(Windows) / SubPC(MacBook) / Cloud(VSCode)
- 証跡URL: https://github.com/owner/repo/issues/123
```

### 将来のサービス移行への備え

- サービス・モデル・端末情報を記録
- 証跡URLで作業履歴を追跡可能
- 知識蓄積でベストプラクティスを継承

## 🧠 知識蓄積システム

### PIMS気づきの記録

```markdown
## 🧠 PIMS気づき（knowledge.mdc反映）
- 気づき: 具体的な発見・学び
- 根拠(Evidence): データ・ログ・結果
- 反映リンク: .cursor/rules/knowledge.mdc
```

### 自動知識抽出

- Issues/PRsクローズ時に自動実行
- PIMS気づきセクションを抽出
- `.cursor/rules/knowledge.mdc`に蓄積

## 🏷️ ラベル運用

### PDCAフェーズラベル
- `pdca-phase:plan`: 計画段階
- `pdca-phase:do`: 実行段階  
- `pdca-phase:check`: 検証段階
- `pdca-phase:act`: 改善段階

### タイプラベル
- `pims-type:development`: 開発・機能追加
- `pims-type:bugfix`: バグ修正
- `pims-type:refactor`: リファクタリング
- `pims-type:documentation`: ドキュメント
- `pims-type:infrastructure`: インフラ・設定

### 優先度ラベル
- `priority:high`: 高優先度
- `priority:medium`: 中優先度
- `priority:low`: 低優先度

### ステータスラベル
- `status:blocked`: ブロック中
- `status:in-progress`: 進行中
- `status:review`: レビュー中
- `status:completed`: 完了

## ⚙️ GitHub Actions活用

### pims-setup.yml
- 初期セットアップ（ラベル作成）
- 手動実行: `gh workflow run pims-setup.yml`

### pims-ci.yml
- PIMS構造検証
- ラベル存在確認
- 知識蓄積チェック

### pims-knowledge.yml
- 知識自動抽出・蓄積
- Issues/PRsクローズ時に実行

## 🔧 GitHub機能の活用

### Pull Request Review
- Evidence（証跡）の確認
- PIMS気づきの妥当性チェック
- 担当者情報の検証

### GitHub Projects
- PDCAフェーズ別ボード作成
- 進捗可視化
- バックログ管理

### GitHub Checks
- CI/CD結果をEvidenceとして記録
- 自動テスト結果の証跡化

## 📊 ベストプラクティス

### ✅ 良い例

**明確な目的設定:**
```
目的: ユーザーが安全にログインできる認証システムを構築
成功基準:
- [ ] JWT認証実装完了
- [ ] パスワードハッシュ化実装
- [ ] セッション管理実装
- [ ] セキュリティテスト通過
```

**具体的な証跡:**
```
Evidence:
- build: https://github.com/owner/repo/actions/runs/12345
- type-check: 0 errors, 0 warnings
- テスト結果: https://github.com/owner/repo/issues/456#issuecomment-789
- スクリーンショット: login-flow.png
```

### ❌ 避けるべき例

- 曖昧な記述: 「改善する」「最適化する」
- 根拠のない主張
- 証跡の記録なし
- 知識の放置

## 🚨 トラブルシューティング

### よくある問題

**Q: ラベルが作成されない**
A: `gh workflow run pims-setup.yml -f setup_labels=true` を実行

**Q: 知識が蓄積されない**
A: `.cursor/rules/knowledge.mdc` ファイルが存在するか確認

**Q: Actionsが実行されない**
A: リポジトリのSettings > Actions > General でActionsを有効化

## 📚 参考資料

- [GitHub Issues ドキュメント](https://docs.github.com/en/issues)
- [GitHub Actions ドキュメント](https://docs.github.com/en/actions)
- [PDCAサイクル - Wikipedia](https://ja.wikipedia.org/wiki/PDCA%E3%82%B5%E3%82%A4%E3%82%AF%E3%83%AB)
