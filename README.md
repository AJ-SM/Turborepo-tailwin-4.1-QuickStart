# Turborepo & Tailwind CSS v4.1 Quick Start

A guide to quickly set up a Turborepo project with a shared Tailwind CSS v4.1 configuration.

This guide uses **NPM**. The key difference from the official Turborepo documentation is the simplification of the Tailwind CSS installation by omitting certain workspace-specific commands.

**Reference:** [Turborepo Docs: Installing Tailwind CSS](https://turborepo.com/docs/guides/tools/tailwind)

---

## Quick Start Guide

Follow these steps to integrate a shared Tailwind CSS configuration into your Turborepo project.

### 1. Initialize Turborepo Project

Start by creating a new Turborepo project.

```bash
npx create-turbo@latest
```

---

### 2. Create Shared Tailwind Config Package

Create a new package to hold the shared Tailwind CSS and PostCSS configuration.

1.  Create the directory: `packages/tailwind-config`

    <img width="276" height="134" alt="Directory structure showing the new tailwind-config folder" src="https://github.com/user-attachments/assets/3c209322-613d-4318-a99b-a23c7ac8782c" />

2.  Inside `packages/tailwind-config`, create the following files:

    <img width="293" height="108" alt="Files inside the tailwind-config folder" src="https://github.com/user-attachments/assets/066238c4-c8c0-478c-a7b7-27ec6c752ebe" />

#### `package.json`

This file defines the package, its dependencies, and exports the config files.

```json
{
  "name": "@repo/tailwind-config",
  "version": "0.0.0",
  "type": "module",
  "private": true,
  "exports": {
    ".": "./shared-styles.css",
    "./postcss": "./postcss.config.js"
  },
  "devDependencies": {
    "postcss": "^8.5.3",
    "tailwindcss": "^4.1.5"
  }
}
```

#### `shared-styles.css`

This file contains your shared Tailwind directives and custom theme variables.

```css
@import 'tailwindcss';

@theme {
  --blue-1000: #2a8af6;
  --purple-1000: #a853ba;
  --red-1000: #e92a67;
}
```

#### `postcss.config.js`

This file exports the shared PostCSS configuration.

```javascript
export const postcssConfig = {
  plugins: {
    '@tailwindcss/postcss': {},
  },
};
```

---

### 3. Configure Project Scripts & Dependencies

Update the configuration files in your project's root directory.

#### Root `turbo.json`

This file manages the build process for your monorepo. The following configuration can be added to your root `turbo.json` to handle the `ui` package tasks.

```json
{
  "extends": ["//"],
  "tasks": {
    "build": {
      "dependsOn": ["build:styles", "build:components"]
    },
    "build:styles": {
      "outputs": ["dist/**"]
    },
    "build:components": {
      "outputs": ["dist/**"]
    },
    "dev": {
      "with": ["dev:styles", "dev:components"]
    },
    "dev:styles": {
      "cache": false,
      "persistent": true
    },
    "dev:components": {
      "cache": false,
      "persistent": true
    }
  }
}
```

#### Root `package.json`

Ensure your root `package.json` includes the new packages in its dependencies. Remember to update the `"name"` field if you replace the whole file.

```json
{
  "name": "my-turborepo",
  "private": true,
  "scripts": {
    "build": "turbo run build",
    "dev": "turbo run dev",
    "lint": "turbo run lint",
    "format": "prettier --write \"**/*.{ts,tsx,md}\"",
    "check-types": "turbo run check-types"
  },
  "devDependencies": {
    "prettier": "^3.6.2",
    "turbo": "^2.5.5",
    "typescript": "5.8.3"
  },
  "engines": {
    "node": ">=18"
  },
  "packageManager": "npm@10.7.0",
  "workspaces": [
    "apps/*",
    "packages/*"
  ],
  "dependencies": {
    "@repo/tailwind-config": "^0.0.0",
    "@repo/ui": "^0.0.0"
  }
}
```

---

### 4. Configure UI Package and Install

Prepare the shared `ui` package and install dependencies.

1.  Create an empty `styles.css` file in `packages/ui/src/`. This file will be populated by the build process.

    <img width="284" height="229" alt="UI package structure with new styles.css file" src="https://github.com/user-attachments/assets/1ffad552-01de-47b2-a899-8bc6decc8af4" />

2.  Run the install command from the root directory to link all workspace packages.

    ```bash
    npm install
    ```

---

### 5. Configure Web Application

Finally, configure your web application (e.g., in `apps/web`) to import and use the shared Tailwind configuration.

1.  **Update `globals.css`**

    In `apps/web/app/globals.css`, import the shared styles from your packages.

    ```css
    @import 'tailwindcss';
    @import '@repo/tailwind-config';
    @import '@repo/ui/styles.css';
    ```

2.  **Add `postcss.config.js`**

    Create a `postcss.config.js` file in the `apps/web` directory to use the shared PostCSS configuration.

    ```javascript
    import { postcssConfig } from '@repo/tailwind-config/postcss';

    export default postcssConfig;
    ```

---

## Completion

Done! Your Turborepo project is now configured to use a shared Tailwind CSS v4.1 setup. You can run your development server to see the changes.
