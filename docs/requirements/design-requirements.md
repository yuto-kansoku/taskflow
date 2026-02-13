# デザイン要件定義書 - TaskFlow

**プロジェクト名**: TaskFlow
**バージョン**: 1.0.0
**作成日**: 2026-02-13
**Phase**: Phase 1 - Requirements

---

## 🎨 美的方向性（Aesthetic Direction）

### トーン（Tone）

**modern-productivity**（モダン・生産性）

- 清潔感のあるミニマルデザイン
- 視覚的ノイズの最小化
- タスク管理に集中できるUI
- プロフェッショナルな印象

### ムード（Mood）

**focused**（集中）

- 落ち着いた配色
- 適度な余白
- 視線誘導の明確さ
- ストレスフリーな操作感

### 差別化（Differentiation）

> "タスク管理は、**見ること**ではなく**完了すること**。UIは邪魔をせず、行動を促す。"

**キーコンセプト**:
- **インビジブルデザイン**: UIが意識されないほどスムーズ
- **アクションファースト**: ボタン・操作が常に見える位置に
- **ビジュアルヒエラルキー**: 重要度が一目でわかる

### 参照サイト・アプリ

| サイト/アプリ | 参考ポイント |
|-------------|------------|
| Linear | タスク操作のスピード感、キーボードショートカット |
| Notion | クリーンな余白使い、タイポグラフィ |
| Todoist | シンプルなタスク表示、優先度の色分け |
| Height | ダッシュボードの情報密度 |

---

## 🎨 ブランドガイドライン

### カラーパレット

#### プライマリカラー

```
Primary: #2563eb (青 - 信頼・生産性)
  - 主要アクションボタン
  - リンク
  - フォーカス状態
```

#### セカンダリカラー

```
Secondary: #64748b (スレートグレー - 落ち着き)
  - 補助的なUI要素
  - ボーダー
  - ディセーブル状態
```

#### アクセントカラー

```
Accent: #10b981 (緑 - 完了・成功)
  - 完了済みタスク
  - 成功メッセージ
  - 進捗バー

Warning: #f59e0b (オレンジ - 注意)
  - 締め切り間近
  - 警告メッセージ

Danger: #ef4444 (赤 - 緊急・削除)
  - 期限切れタスク
  - 削除アクション
  - エラーメッセージ
```

#### 背景・テキスト

```
Background:
  - Light: #ffffff (白)
  - Subtle: #f8fafc (淡いグレー)
  - Dark: #0f172a (ダークモード背景)

Text:
  - Primary: #0f172a (濃いスレート)
  - Secondary: #64748b (スレートグレー)
  - Muted: #94a3b8 (淡いスレート)
  - Inverse: #ffffff (白 - ダークモード用)
```

### タイポグラフィ

#### フォント選択

**Heading**: **Inter** (Modern, Clean, Professional)
- 見出し・ボタンテキスト
- ウェイト: 500 (Medium), 600 (SemiBold), 700 (Bold)

**Body**: **Inter** (統一感)
- 本文・説明文
- ウェイト: 400 (Regular), 500 (Medium)

**Monospace**: **JetBrains Mono** (コード表示用)
- タスクID表示
- 技術情報

#### サイズスケール

| 用途 | サイズ | ウェイト | 行間 |
|------|--------|---------|------|
| Display | 3rem (48px) | 700 | 1.2 |
| H1 | 2.25rem (36px) | 600 | 1.3 |
| H2 | 1.875rem (30px) | 600 | 1.3 |
| H3 | 1.5rem (24px) | 600 | 1.4 |
| H4 | 1.25rem (20px) | 600 | 1.4 |
| Body Large | 1.125rem (18px) | 400 | 1.6 |
| Body | 1rem (16px) | 400 | 1.6 |
| Body Small | 0.875rem (14px) | 400 | 1.5 |
| Caption | 0.75rem (12px) | 500 | 1.4 |

---

## 🧩 UIコンポーネント方針

### コンポーネントライブラリ

**shadcn/ui** (Radix UI ベース)

**選定理由**:
- アクセシビリティ完全対応（WCAG AA）
- 柔軟なカスタマイズ性
- TypeScript完全サポート
- コンポーネントをソースコードとして管理

### スタイリング

**Tailwind CSS v3.4+**

**設定**:
- カスタムカラーパレット定義
- カスタムスペーシングスケール
- ダークモード対応（class戦略）

### アニメーション

**Motion/React (Framer Motion)**

**原則**:
- **200ms以下**の高速インタラクション
- **Compositor properties only** (transform, opacity)
- レイアウトプロパティのアニメーション禁止
- reduced-motion対応必須

**使用例**:
```tsx
// ✅ Good - Compositor properties
<motion.div
  initial={{ opacity: 0, y: 10 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.15 }}
/>

// ❌ Bad - Layout properties
<motion.div
  animate={{ height: 'auto' }} // 禁止
/>
```

### アイコン

**Lucide React**

**原則**:
- 一貫したストローク幅（2px）
- 24×24px基準
- アクセシブルなラベル付与

---

## 📱 レスポンシブ方針

### モバイルファースト

**デザイン順序**: Mobile → Tablet → Desktop

### ブレークポイント

| サイズ | 幅 | 用途 |
|--------|-----|------|
| sm | 640px | スマートフォン（縦） |
| md | 768px | タブレット（縦） |
| lg | 1024px | タブレット（横）、小型ノートPC |
| xl | 1280px | デスクトップ |
| 2xl | 1536px | 大型デスクトップ |

### レイアウトパターン

**モバイル（< 768px）**:
- 単一カラムレイアウト
- ハンバーガーメニュー
- フローティングアクションボタン（FAB）
- スワイプジェスチャー対応

**タブレット（768px - 1024px）**:
- 2カラムレイアウト（リスト + 詳細）
- サイドバー（折りたたみ可能）

**デスクトップ（1024px+）**:
- 3カラムレイアウト（サイドバー + リスト + 詳細）
- キーボードショートカット強化
- ホバー状態の活用

### タッチターゲット

**最小サイズ**: **44×44px**（WCAG 2.1準拠）

---

## ✨ 品質基準

### Lighthouse目標

| 項目 | 目標値 | 許容下限 |
|------|--------|----------|
| Performance | **90+** | 85 |
| Accessibility | **100** | 100（必達） |
| Best Practices | **100** | 95 |
| SEO | **100** | 90 |

### Core Web Vitals

| 指標 | 目標値 | 許容上限 |
|------|--------|----------|
| FCP (First Contentful Paint) | < 1.5s | < 1.8s |
| LCP (Largest Contentful Paint) | < 2.0s | < 2.5s |
| CLS (Cumulative Layout Shift) | < 0.05 | < 0.1 |
| FID (First Input Delay) | < 50ms | < 100ms |
| INP (Interaction to Next Paint) | < 100ms | < 200ms |

### アクセシビリティ

**WCAG 2.1 Level AA準拠（必達）**

| 項目 | 基準 |
|------|------|
| コントラスト比 | 4.5:1以上（通常テキスト） |
| キーボード操作 | すべての機能が操作可能 |
| スクリーンリーダー | aria-label完備 |
| フォーカス表示 | 明確なフォーカスリング |
| エラーメッセージ | 明示的・具体的 |

### ダークモード

**必須対応**

- システム設定の自動検出
- 手動切り替え可能
- LocalStorageで設定保存
- スムーズな切り替えアニメーション

---

## 🚫 Anti-Patterns（禁止事項）

以下は**絶対に使用しない**:

### フォント

❌ **Roboto** - AI slop
❌ **Arial** - AI slop
❌ **Comic Sans** - プロフェッショナルではない

✅ **Inter** を使用

### カラー

❌ **紫グラデーション on 白背景** - AI slop
❌ **過剰なドロップシャドウ** - AI slop
❌ **グロー効果** - AI slop
❌ **虹色グラデーション** - 視覚的ノイズ

✅ シンプルな単色・2色グラデーション

### レイアウト

❌ **予測可能な3カラムレイアウト** - 差別化不足
❌ **センター寄せのカードグリッド** - AI slop

✅ ダイナミックなレイアウト

### エフェクト

❌ **blur > 10px** - パフォーマンス劣化
❌ **レイアウトプロパティのアニメーション** - ジャンク発生
❌ **過剰なパララックス** - 酔う

✅ 高速・シンプル・目的のあるアニメーション

---

## 🎯 UI実装原則

### 1. Purpose First（目的優先）

> "このUIは何を達成するために存在するのか？"

- 装飾のための装飾は禁止
- 機能に基づくデザイン
- ユーザーの目的達成を最優先

### 2. Invisible UI（透明なUI）

> "最高のUIは、ユーザーが気づかないUI"

- 直感的な配置
- 一貫した操作パターン
- 学習コスト最小化

### 3. Speed Matters（速度が重要）

> "1秒の遅延は、1%のコンバージョン損失"

- 200ms以内のインタラクション
- Optimistic UI Update
- スケルトンスクリーン活用

### 4. Accessibility is Not Optional（アクセシビリティは必須）

> "すべての人が使えるUIが、良いUI"

- WCAG AA準拠は最低ライン
- キーボード操作完全対応
- スクリーンリーダーテスト必須

---

## 📦 実装ツールチェーン

| カテゴリ | ツール | バージョン |
|---------|--------|-----------|
| フレームワーク | Next.js | 15.x |
| スタイリング | Tailwind CSS | 3.4+ |
| コンポーネント | shadcn/ui | latest |
| アニメーション | Motion/React | 11.x |
| アイコン | Lucide React | latest |
| フォーム | React Hook Form | 7.x |
| バリデーション | Zod | 3.x |
| 日時操作 | date-fns | 3.x |
| 状態管理 | Zustand | 4.x |

---

## 📊 デザインシステム参照

Phase 2で生成される `design-system.yml` は、本ドキュメントをベースに以下を含みます：

- トークン化されたカラーパレット
- タイポグラフィスケール
- スペーシングスケール
- コンポーネント仕様
- レスポンシブブレークポイント

---

## 🔗 関連ドキュメント

- [要件定義書](./requirements.md) - 機能要件・非機能要件
- [design-system.yml](../design/design-system.yml) - Phase 2で生成
- [ui-guidelines.md](../design/ui-guidelines.md) - Phase 2で生成
- [component-library.md](../design/component-library.md) - Phase 2で生成

---

## 📅 次のステップ

Phase 2で以下のコマンドを実行：

```bash
# デザインシステム生成
/design-system

# フロントエンド設計方針
/frontend-design

# テスト設計書生成
/generate-test-design
```

---

🤖 **Generated by CCAGI SDK v3.5.0**
📅 **作成日**: 2026-02-13
📝 **Phase**: 1 - Requirements
