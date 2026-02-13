# コンポーネントライブラリ設定 - TaskFlow

**プロジェクト**: TaskFlow
**バージョン**: 1.0.0
**生成日**: 2026-02-13
**Phase**: Phase 2 - Design

---

## 📦 コンポーネントライブラリ

### 選定: shadcn/ui + Radix UI

**選定理由**:
- ✅ **アクセシビリティ完全対応** (WCAG AA)
- ✅ **TypeScript完全サポート**
- ✅ **コンポーネントをソースコードとして管理** (依存地獄回避)
- ✅ **柔軟なカスタマイズ性**
- ✅ **Tailwind CSS統合**

---

## 🛠️ セットアップ手順

### Step 1: shadcn/ui初期化

```bash
npx shadcn@latest init
```

**プロンプト選択**:
```
✔ Would you like to use TypeScript? … yes
✔ Which style would you like to use? › New York
✔ Which color would you like to use as base color? › Slate
✔ Where is your global CSS file? … src/styles/globals.css
✔ Would you like to use CSS variables for colors? … yes
✔ Are you using a custom tailwind prefix? … no
✔ Where is your tailwind.config.js located? … tailwind.config.ts
✔ Configure the import alias for components? … @/components
✔ Configure the import alias for utils? … @/lib/utils
✔ Are you using React Server Components? … yes
```

### Step 2: コアコンポーネントインストール

```bash
# ボタン・フォーム系
npx shadcn@latest add button
npx shadcn@latest add input
npx shadcn@latest add textarea
npx shadcn@latest add select
npx shadcn@latest add checkbox
npx shadcn@latest add radio-group
npx shadcn@latest add switch
npx shadcn@latest add label
npx shadcn@latest add form

# レイアウト系
npx shadcn@latest add card
npx shadcn@latest add separator
npx shadcn@latest add scroll-area
npx shadcn@latest add tabs
npx shadcn@latest add accordion

# オーバーレイ系
npx shadcn@latest add dialog
npx shadcn@latest add sheet
npx shadcn@latest add popover
npx shadcn@latest add dropdown-menu
npx shadcn@latest add tooltip

# フィードバック系
npx shadcn@latest add toast
npx shadcn@latest add alert
npx shadcn@latest add badge
npx shadcn@latest add progress
npx shadcn@latest add skeleton

# データ表示系
npx shadcn@latest add table
npx shadcn@latest add avatar
npx shadcn@latest add calendar
npx shadcn@latest add command
```

---

## 🎨 カスタマイズ

### colors.css

`src/styles/colors.css` を作成:

```css
@layer base {
  :root {
    /* Primary */
    --primary: 221 83% 53%;  /* #2563eb */
    --primary-foreground: 210 40% 98%;

    /* Secondary */
    --secondary: 215 16% 47%;  /* #64748b */
    --secondary-foreground: 210 40% 98%;

    /* Success */
    --success: 160 84% 39%;  /* #10b981 */
    --success-foreground: 210 40% 98%;

    /* Warning */
    --warning: 38 92% 50%;  /* #f59e0b */
    --warning-foreground: 210 40% 98%;

    /* Danger */
    --danger: 0 84% 60%;  /* #ef4444 */
    --danger-foreground: 210 40% 98%;

    /* Background */
    --background: 0 0% 100%;  /* #ffffff */
    --foreground: 222 47% 11%;  /* #0f172a */

    /* Card */
    --card: 0 0% 100%;
    --card-foreground: 222 47% 11%;

    /* Border */
    --border: 214 32% 91%;  /* #e2e8f0 */
    --input: 214 32% 91%;
    --ring: 221 83% 53%;  /* focus ring */

    /* Muted */
    --muted: 210 40% 96%;  /* #f1f5f9 */
    --muted-foreground: 215 16% 47%;  /* #64748b */

    /* Accent */
    --accent: 210 40% 96%;
    --accent-foreground: 222 47% 11%;

    /* Radius */
    --radius: 0.5rem;  /* 8px */
  }

  .dark {
    --background: 222 47% 11%;  /* #0f172a */
    --foreground: 210 40% 98%;

    --card: 222 47% 11%;
    --card-foreground: 210 40% 98%;

    --border: 217 33% 17%;
    --input: 217 33% 17%;

    --muted: 217 33% 17%;
    --muted-foreground: 215 16% 47%;

    --accent: 217 33% 17%;
    --accent-foreground: 210 40% 98%;
  }
}
```

---

## 🔧 Tailwind設定

### tailwind.config.ts

```typescript
import type { Config } from "tailwindcss"

const config = {
  darkMode: ["class"],
  content: [
    './pages/**/*.{ts,tsx}',
    './components/**/*.{ts,tsx}',
    './app/**/*.{ts,tsx}',
    './src/**/*.{ts,tsx}',
  ],
  prefix: "",
  theme: {
    container: {
      center: true,
      padding: "2rem",
      screens: {
        "2xl": "1400px",
      },
    },
    extend: {
      colors: {
        border: "hsl(var(--border))",
        input: "hsl(var(--input))",
        ring: "hsl(var(--ring))",
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))",
        },
        secondary: {
          DEFAULT: "hsl(var(--secondary))",
          foreground: "hsl(var(--secondary-foreground))",
        },
        success: {
          DEFAULT: "hsl(var(--success))",
          foreground: "hsl(var(--success-foreground))",
        },
        warning: {
          DEFAULT: "hsl(var(--warning))",
          foreground: "hsl(var(--warning-foreground))",
        },
        danger: {
          DEFAULT: "hsl(var(--danger))",
          foreground: "hsl(var(--danger-foreground))",
        },
        muted: {
          DEFAULT: "hsl(var(--muted))",
          foreground: "hsl(var(--muted-foreground))",
        },
        accent: {
          DEFAULT: "hsl(var(--accent))",
          foreground: "hsl(var(--accent-foreground))",
        },
        card: {
          DEFAULT: "hsl(var(--card))",
          foreground: "hsl(var(--card-foreground))",
        },
      },
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },
      keyframes: {
        "accordion-down": {
          from: { height: "0" },
          to: { height: "var(--radix-accordion-content-height)" },
        },
        "accordion-up": {
          from: { height: "var(--radix-accordion-content-height)" },
          to: { height: "0" },
        },
      },
      animation: {
        "accordion-down": "accordion-down 0.2s ease-out",
        "accordion-up": "accordion-up 0.2s ease-out",
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
} satisfies Config

export default config
```

---

## 📋 コンポーネント使用例

### Button

```tsx
import { Button } from "@/components/ui/button"

// Primary
<Button>タスクを作成</Button>

// Secondary
<Button variant="secondary">キャンセル</Button>

// Success
<Button className="bg-success hover:bg-success/90">完了</Button>

// Danger
<Button variant="destructive">削除</Button>

// Icon Button
<Button size="icon">
  <PlusIcon className="h-4 w-4" />
</Button>
```

### Card

```tsx
import { Card, CardHeader, CardTitle, CardContent } from "@/components/ui/card"

<Card>
  <CardHeader>
    <CardTitle>今日のタスク</CardTitle>
  </CardHeader>
  <CardContent>
    {tasks.map(task => <TaskItem key={task.id} task={task} />)}
  </CardContent>
</Card>
```

### Dialog

```tsx
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog"

<Dialog>
  <DialogTrigger asChild>
    <Button>新規タスク</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>タスクを作成</DialogTitle>
    </DialogHeader>
    <TaskForm />
  </DialogContent>
</Dialog>
```

---

## ✅ チェックリスト

Phase 4実装前に確認：

- [ ] shadcn/ui初期化完了
- [ ] コアコンポーネント16個インストール
- [ ] colors.css作成
- [ ] tailwind.config.ts設定
- [ ] cn utility関数確認
- [ ] ダークモード動作確認

---

## 🔗 関連ドキュメント

- [design-system.yml](./design-system.yml) - デザインシステム定義
- [responsive-guidelines.md](./responsive-guidelines.md) - レスポンシブガイドライン
- [shadcn/ui公式ドキュメント](https://ui.shadcn.com)
- [Radix UI公式ドキュメント](https://www.radix-ui.com)

---

🤖 **Generated by CCAGI SDK v3.5.0**
📅 **生成日**: 2026-02-13
📝 **Phase**: 2 - Design
