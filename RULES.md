# CLAUDE.md — Agent Instructions for Coffee Webapp

> This file is automatically read by Claude Code at the start of every session.
> Keep it up-to-date as the project evolves.

---

## Project Overview

A coffee-focused web application built with modern React/Next.js.
**Goal:** [Describe your app's core purpose — e.g., "Browse menus, order coffee, track loyalty points"]

---

## Tech Stack

| Layer       | Technology                                      |
|-------------|------------------------------------------------|
| Framework   | Next.js (App Router or Page Router — specify!) |
| UI Library  | React 19                                       |
| Language    | TypeScript (strict mode)                       |
| Styling     | Tailwind CSS v4 + shadcn/ui                    |
| Forms       | shadcn/ui Form + React Hook Form + Zod         |
| Validation  | Zod                                            |
| Runtime     | Bun                                            |

---

## Project Structure

```
src/
├── app/                  # Next.js App Router pages & layouts
├── components/
│   ├── ui/               # shadcn/ui primitives (DO NOT edit manually)
│   ├── common/           # Shared generic components (Flex, Container, etc.)
│   └── features/         # Feature-specific components (menu/, cart/, auth/)
├── lib/
│   ├── schemas/          # Zod schemas
│   └── utils.ts          # cn() and shared utilities
├── hooks/                # Custom React hooks
└── types/                # Global TypeScript types
```

---

## Component Rules

### 1. Prefer shadcn/ui primitives
Always reach for existing shadcn components before writing custom ones.
```tsx
// ✅ Good
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"

// ❌ Bad — don't reinvent primitives
<button className="px-4 py-2 bg-blue-500 rounded">...</button>
```

### 2. Flex component — use when repeating flex layout
Whenever you find yourself repeating `flex items-center gap-*` or similar
combinations more than twice, use (or create) the shared `<Flex>` component.

```tsx
// src/components/common/Flex.tsx
import { cn } from "@/lib/utils"
import { HTMLAttributes } from "react"

type FlexProps = HTMLAttributes<HTMLDivElement> & {
  direction?: "row" | "col"
  align?: "start" | "center" | "end" | "between" | "stretch"
  justify?: "start" | "center" | "end" | "between"
  gap?: number   // maps to gap-{n} Tailwind class
  wrap?: boolean
}

export function Flex({
  direction = "row",
  align = "center",
  justify = "start",
  gap = 2,
  wrap = false,
  className,
  children,
  ...props
}: FlexProps) {
  return (
    <div
      className={cn(
        "flex",
        direction === "col" && "flex-col",
        `items-${align}`,
        `justify-${justify}`,
        `gap-${gap}`,
        wrap && "flex-wrap",
        className
      )}
      {...props}
    >
      {children}
    </div>
  )
}
```

Usage:
```tsx
<Flex align="center" gap={4}>
  <CoffeeIcon />
  <span>Latte</span>
</Flex>
```

### 3. New common components
When a pattern is repeated 3+ times across features, extract it to `components/common/`.
Name clearly: `PageHeader`, `SectionTitle`, `EmptyState`, `LoadingSpinner`, etc.

---

## Form Rules

All forms must use the **shadcn Form + Zod** pattern:

```tsx
// 1. Define schema
const orderSchema = z.object({
  item: z.string().min(1, "Please select an item"),
  quantity: z.number().int().min(1).max(10),
  notes: z.string().max(200).optional(),
})

type OrderFormValues = z.infer<typeof orderSchema>

// 2. Use shadcn Form
import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { Form, FormField, FormItem, FormLabel, FormMessage } from "@/components/ui/form"

export function OrderForm() {
  const form = useForm<OrderFormValues>({
    resolver: zodResolver(orderSchema),
    defaultValues: { quantity: 1 },
  })

  function onSubmit(values: OrderFormValues) { /* ... */ }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)}>
        <FormField
          control={form.control}
          name="item"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Item</FormLabel>
              <Input {...field} />
              <FormMessage />
            </FormItem>
          )}
        />
      </form>
    </Form>
  )
}
```

---

## TypeScript Rules

- Always use `type` over `interface` unless declaration merging is needed.
- Prefer `z.infer<typeof schema>` for form/API types — single source of truth.
- Avoid `any`. Use `unknown` + type narrowing when the type is uncertain.
- Use `== null` to check both `null` and `undefined` (project convention).

```ts
// ✅ Concise null check
if (value == null) return

// ❌ Verbose
if (value === null || value === undefined) return
```

---

## Styling Rules

- **Tailwind only** — no inline styles, no CSS modules (unless absolutely necessary).
- Use `cn()` from `@/lib/utils` to merge conditional classes:
  ```tsx
  import { cn } from "@/lib/utils"
  <div className={cn("base-class", isActive && "active-class", className)} />
  ```
- Follow the shadcn/ui color token system: `bg-background`, `text-foreground`,
  `text-muted-foreground`, `border`, `ring`, etc. — don't hardcode hex colors.
- Dark mode is supported via `dark:` variants (shadcn handles it via CSS variables).

---

## File & Naming Conventions

| Item               | Convention              | Example                        |
|--------------------|-------------------------|-------------------------------|
| Components         | PascalCase              | `CoffeeCard.tsx`              |
| Hooks              | camelCase, `use` prefix | `useCoffeeMenu.ts`            |
| Zod schemas        | camelCase + `Schema`    | `orderSchema`                 |
| Types (inferred)   | PascalCase + `Values`   | `OrderFormValues`             |
| Page files         | lowercase/kebab         | `app/menu/page.tsx`           |
| Utility functions  | camelCase               | `formatPrice.ts`              |

---

## Running the Project

```bash
bun install          # Install dependencies
bun dev              # Start dev server
bun build            # Production build
bun lint             # Lint
bun typecheck        # Type check (tsc --noEmit)
```

---

## Adding a New shadcn Component

```bash
bunx --bun shadcn@latest add <component-name>
# e.g.
bunx --bun shadcn@latest add dialog
bunx --bun shadcn@latest add select
```

> Never manually edit files inside `src/components/ui/` — re-run the add command instead.

---

## Agent Workflow Guidelines

When implementing a new feature, follow this order:

1. **Schema first** — define the Zod schema in `lib/schemas/`
2. **Types** — infer TypeScript types from the schema
3. **Component** — build UI using shadcn primitives + `<Flex>` where appropriate
4. **Form** (if needed) — wire up shadcn Form + zodResolver
5. **Review** — check for repeated className patterns → extract to `components/common/`

When in doubt about a component:
- Check `src/components/common/` first
- Check shadcn/ui docs at https://ui.shadcn.com/docs/components
- Then build new

---

## Out of Scope (Don't Add Without Discussion)

- New CSS frameworks or UI libraries
- `styled-components` or CSS-in-JS
- Redux / Zustand (discuss state management approach first)
- Direct DOM manipulation

---

*Last updated: April 2026*
