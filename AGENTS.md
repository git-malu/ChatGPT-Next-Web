# Agent Guidelines for NextChat (ChatGPT-Next-Web)

Welcome, agent. This document outlines the technical standards, commands, and patterns used in this repository. Adhere to these guidelines to maintain consistency and quality.

## 🚀 Commands

### Build & Development

- **Install:** `yarn install`
- **Dev (Web):** `yarn dev`
- **Dev (Desktop/Tauri):** `yarn app:dev`
- **Build (Web):** `yarn build`
- **Build (Desktop/Tauri):** `yarn app:build`
- **Lint:** `yarn lint` (uses Next.js linting with Prettier and unused-imports plugin)

### Testing

- **Run all tests (watch):** `yarn test`
- **Run all tests (CI):** `yarn test:ci`
- **Run single test:** `node --no-warnings --experimental-vm-modules $(yarn bin jest) <path-to-file>`
  - _Example:_ `node --no-warnings --experimental-vm-modules $(yarn bin jest) test/sum-module.test.ts`

---

## 📂 Project Structure

- `app/`: Next.js App Router root.
  - `components/`: UI components with `*.module.scss` styles.
  - `store/`: Zustand state management with persistence.
  - `client/`: LLM provider implementations (OpenAI, Gemini, Claude, etc.).
  - `locales/`: I18n localization files.
  - `styles/`: Global styles and variables.
  - `utils/`: Shared utility functions.
- `src-tauri/`: Rust code and config for the Tauri desktop app.
- `test/`: Jest test files.

---

## 💻 Coding Standards

### 1. TypeScript & Types

- **Strict Mode:** TypeScript strict mode is enabled. Avoid `any` whenever possible.
- **Definitions:** Prefer `interface` for component props and `type` for complex unions or object shapes.
- **Enums:** Use `enum` for sets of related constants (e.g., `ServiceProvider`, `Theme`).
- **Typings:** Shared types should go in `app/typing.ts` or `app/client/api.ts`.

### 2. Naming Conventions

- **Components:** `PascalCase` (e.g., `IconButton.tsx`).
- **Functions/Variables:** `camelCase` (e.g., `copyToClipboard`).
- **Constants:** `UPPER_SNAKE_CASE` (e.g., `REQUEST_TIMEOUT_MS`).
- **Styles:** `kebab-case` for CSS classes in SCSS modules.

### 3. Imports

- **Absolute Paths:** Use the `@/` alias for absolute imports from the project root.
  - _Example:_ `import { useAccessStore } from "@/app/store";`
- **Relative Paths:** Use relative paths for files within the same directory or small subdirectories.

### 4. React & Styling

- **Functional Components:** Always use functional components and hooks.
- **Styles:** Use SCSS Modules (`*.module.scss`). Combine classes with `clsx`.
- **Icons:** Use the custom icons in `app/icons/` or passed as `JSX.Element`.

### 5. State Management

- **Zustand:** All global state must use Zustand.
- **Persistence:** Use `createPersistStore` (a wrapper around `persist`) for state that should survive reloads.
- **Access:** Use `Store.getState()` for non-reactive access outside components, and `useStore()` hooks inside components.

### 6. Error Handling & Feedback

- **Try/Catch:** Wrap async operations (API calls, file I/O) in `try...catch`.
- **User Feedback:** Use `showToast(Locale.Error.Message)` to notify users of failures.
- **Logging:** Use `console.error` for developer-facing errors.

### 7. Localization (I18n)

- **Never Hardcode Strings:** Use the `Locale` object from `@/app/locales`.
- **Usage:** `Locale.Component.Field`.

---

## 🛠 Features & Integrations

### Tauri (Desktop)

- Detect Tauri environment using `window.__TAURI__`.
- Use Tauri-specific APIs for file system access and dialogs when available.

### MCP (Model Context Protocol)

- MCP logic resides in `app/mcp/`.
- Enable via `ENABLE_MCP=true` environment variable.

### Artifacts

- Code generation and preview logic is handled in `app/components/artifacts.tsx`.

---

## 📝 Formatting (Prettier)

- `printWidth: 80`
- `tabWidth: 2`
- `semi: true`
- `singleQuote: false`
- `trailingComma: "all"`
- `bracketSpacing: true`
- `arrowParens: "always"`
