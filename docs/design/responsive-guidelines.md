# レスポンシブデザインガイドライン - TaskFlow

**プロジェクト**: TaskFlow
**バージョン**: 1.0.0
**生成日**: 2026-02-13
**Phase**: Phase 2 - Design

---

## 📱 モバイルファースト原則

TaskFlowは**モバイルファースト**でデザインします。

```
Mobile → Tablet → Desktop
```

この順序でデザイン・実装することで：
- ✅ 本質的な機能に集中
- ✅ パフォーマンス最適化
- ✅ プログレッシブエンハンスメント

---

## 📐 ブレークポイント

| サイズ | 幅 | 用途 | レイアウト |
|--------|-----|------|-----------|
| **sm** | 640px | スマートフォン（縦） | 1カラム |
| **md** | 768px | タブレット（縦） | 2カラム |
| **lg** | 1024px | タブレット（横）、小型ノートPC | 2-3カラム |
| **xl** | 1280px | デスクトップ | 3カラム |
| **2xl** | 1536px | 大型デスクトップ | 3カラム（余白増） |

### Tailwind CSSクラス

```tsx
// モバイルファースト
<div className="w-full md:w-1/2 lg:w-1/3">
  {/* モバイル: 100%, タブレット: 50%, デスクトップ: 33% */}
</div>

// ブレークポイント別表示/非表示
<nav className="hidden md:block">
  {/* タブレット以上で表示 */}
</nav>

<button className="md:hidden">
  {/* モバイルのみ表示 */}
  <MenuIcon />
</button>
```

---

## 📱 モバイル（< 768px）

### レイアウト

```
┌─────────────────────┐
│   Header (固定)      │
├─────────────────────┤
│                     │
│   Main Content      │
│   (スクロール)        │
│                     │
│                     │
│                     │
│                     │
│                     │
│                     │
│                     │
├─────────────────────┤
│   FAB (右下固定)     │
└─────────────────────┘
```

### 特徴

- **単一カラム**レイアウト
- **ハンバーガーメニュー**
- **フローティングアクションボタン (FAB)**
- **スワイプジェスチャー**対応
  - 左スワイプ: タスク完了
  - 右スワイプ: タスク削除

### 実装例

```tsx
// モバイルレイアウト
export default function MobileLayout({ children }: PropsWithChildren) {
  return (
    <div className="flex flex-col h-screen">
      {/* Header - 固定 */}
      <header className="sticky top-0 z-50 bg-background border-b px-4 py-3 md:hidden">
        <div className="flex items-center justify-between">
          <button>
            <MenuIcon className="h-6 w-6" />
          </button>
          <h1 className="text-lg font-semibold">TaskFlow</h1>
          <button>
            <SearchIcon className="h-6 w-6" />
          </button>
        </div>
      </header>

      {/* Main Content - スクロール可能 */}
      <main className="flex-1 overflow-y-auto p-4">
        {children}
      </main>

      {/* FAB - 右下固定 */}
      <button className="fixed bottom-6 right-6 md:hidden
                         h-14 w-14 rounded-full
                         bg-primary text-primary-foreground
                         shadow-lg hover:shadow-xl
                         transition-shadow">
        <PlusIcon className="h-6 w-6" />
      </button>
    </div>
  )
}
```

### タッチターゲット

**最小サイズ**: **44×44px** (WCAG 2.1準拠)

```tsx
// ✅ Good - 44px以上
<button className="h-12 w-12"> {/* 48px */}
  <Icon />
</button>

// ❌ Bad - 44px未満
<button className="h-8 w-8"> {/* 32px - 小さすぎる */}
  <Icon />
</button>
```

---

## 📱 タブレット（768px - 1024px）

### レイアウト

```
┌─────────────────────────────────┐
│   Header                         │
├───────────┬─────────────────────┤
│           │                     │
│  Sidebar  │   Main Content      │
│  (折畳可)  │   (スクロール)        │
│           │                     │
│  - Home   │   ┌──────────┐     │
│  - Tasks  │   │  Card 1  │     │
│  - Teams  │   └──────────┘     │
│           │   ┌──────────┐     │
│           │   │  Card 2  │     │
│           │   └──────────┘     │
│           │                     │
└───────────┴─────────────────────┘
```

### 特徴

- **2カラムレイアウト** (サイドバー + メイン)
- サイドバー**折りたたみ可能**
- タッチ・マウス両対応

### 実装例

```tsx
// タブレットレイアウト
export default function TabletLayout({ children }: PropsWithChildren) {
  const [sidebarOpen, setSidebarOpen] = useState(true)

  return (
    <div className="flex h-screen">
      {/* Sidebar - 折りたたみ可能 */}
      <aside className={cn(
        "hidden md:block transition-all duration-200",
        sidebarOpen ? "w-64" : "w-16"
      )}>
        <nav className="p-4">
          {/* Navigation items */}
        </nav>
      </aside>

      {/* Main */}
      <div className="flex-1 flex flex-col">
        <header className="border-b px-6 py-4">
          <button onClick={() => setSidebarOpen(!sidebarOpen)}
                  className="hidden md:inline-flex">
            <MenuIcon />
          </button>
        </header>
        <main className="flex-1 overflow-y-auto p-6">
          {children}
        </main>
      </div>
    </div>
  )
}
```

---

## 💻 デスクトップ（1024px+）

### レイアウト

```
┌────────────────────────────────────────────┐
│   Header                                    │
├──────────┬───────────────┬─────────────────┤
│          │               │                 │
│ Sidebar  │   Task List   │  Task Detail    │
│  (固定)   │   (スクロール)  │  (スクロール)    │
│          │               │                 │
│ - Home   │ ┌──────────┐  │  📝 Task Title  │
│ - Tasks  │ │ Task 1 ✓ │  │                 │
│ - Teams  │ └──────────┘  │  Description... │
│ - Tags   │ ┌──────────┐  │                 │
│          │ │ Task 2   │◀─┼─ Selected      │
│          │ └──────────┘  │                 │
│          │ ┌──────────┐  │  Comments       │
│          │ │ Task 3   │  │  - Comment 1    │
│          │ └──────────┘  │  - Comment 2    │
│          │               │                 │
└──────────┴───────────────┴─────────────────┘
```

### 特徴

- **3カラムレイアウト** (サイドバー + リスト + 詳細)
- サイドバー常時表示
- **キーボードショートカット**強化
- **ホバー状態**の活用

### 実装例

```tsx
// デスクトップレイアウト
export default function DesktopLayout({ children }: PropsWithChildren) {
  return (
    <div className="hidden lg:flex h-screen">
      {/* Sidebar - 常時表示 */}
      <aside className="w-64 border-r bg-muted/40">
        <nav className="p-4">
          {/* Navigation items */}
        </nav>
      </aside>

      {/* Main Content - 3カラム */}
      <div className="flex-1 flex">
        {/* Task List */}
        <div className="w-96 border-r overflow-y-auto">
          <TaskList />
        </div>

        {/* Task Detail */}
        <div className="flex-1 overflow-y-auto">
          <TaskDetail />
        </div>
      </div>
    </div>
  )
}
```

### キーボードショートカット

| ショートカット | 動作 |
|--------------|------|
| `Ctrl/Cmd + N` | 新規タスク作成 |
| `Ctrl/Cmd + K` | コマンドパレット |
| `Ctrl/Cmd + /` | ショートカット一覧 |
| `j / k` | 次/前のタスク |
| `Enter` | タスク詳細表示 |
| `Esc` | 閉じる |

---

## 🎨 レスポンシブコンポーネント例

### タスクカード

```tsx
// TaskCard.tsx
export function TaskCard({ task }: { task: Task }) {
  return (
    <Card className={cn(
      // モバイル: フル幅
      "w-full",
      // タブレット: 2カラムグリッド内
      "md:w-full",
      // デスクトップ: コンパクト表示
      "lg:w-auto"
    )}>
      <CardHeader className="space-y-1">
        <div className="flex items-center justify-between">
          <CardTitle className={cn(
            // モバイル: 小さめ
            "text-base",
            // タブレット以上: 標準
            "md:text-lg"
          )}>
            {task.title}
          </CardTitle>

          {/* デスクトップのみ表示 */}
          <Badge className="hidden lg:inline-flex">
            {task.priority}
          </Badge>
        </div>
      </CardHeader>

      <CardContent>
        {/* モバイル: 1行省略 */}
        <p className="line-clamp-1 md:line-clamp-2 text-sm text-muted-foreground">
          {task.description}
        </p>

        {/* タブレット以上で表示 */}
        <div className="hidden md:flex items-center gap-2 mt-3">
          <Avatar size="sm" src={task.assignee.avatar} />
          <span className="text-xs">{task.assignee.name}</span>
        </div>
      </CardContent>
    </Card>
  )
}
```

---

## ✅ チェックリスト

Phase 4実装時に確認：

- [ ] モバイルファースト順序で実装
- [ ] 全ブレークポイントで動作確認
- [ ] タッチターゲット44px以上
- [ ] キーボードナビゲーション（デスクトップ）
- [ ] ホバー状態（デスクトップのみ）
- [ ] スワイプジェスチャー（モバイル）
- [ ] FAB配置（モバイル）
- [ ] 横画面対応（モバイル/タブレット）

---

## 🔗 関連ドキュメント

- [design-system.yml](./design-system.yml) - ブレークポイント定義
- [component-library.md](./component-library.md) - コンポーネント実装
- [WCAG 2.1 Touch Target Size](https://www.w3.org/WAI/WCAG21/Understanding/target-size.html)

---

🤖 **Generated by CCAGI SDK v3.5.0**
📅 **生成日**: 2026-02-13
📝 **Phase**: 2 - Design
