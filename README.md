# Turborepo-tailwin-4.1-QuickStart
Get Started Faster and Save the God daam 25 hrs of labour..

#### Using NPM !!! 



[references:]([https://example.com](https://turborepo.com/docs/guides/tools/tailwind)).
### What special i did here is just remove the all of the workspace lines form the tailwind installation code and rest is same. ! 


 
## QuickStart Guid
- Initiate Turborepo porject using
   `npx create-turbo@latest`.

  
- Make tailwind-config folder inside packages.
  <img width="276" height="134" alt="image" src="https://github.com/user-attachments/assets/3c209322-613d-4318-a99b-a23c7ac8782c" />


- Make these files inside tailwind-config file.
  <img width="293" height="108" alt="image" src="https://github.com/user-attachments/assets/066238c4-c8c0-478c-a7b7-27ec6c752ebe" />

  >package.json
  `{
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
  }`
  >shared-styles.css
  `@import 'tailwindcss';
 
  @theme {
  --blue-1000: #2a8af6;
  --purple-1000: #a853ba;
  --red-1000: #e92a67;
  }`

  >postcss.config.js
  `export const postcssConfig = {
  plugins: {
    '@tailwindcss/postcss': {},
  },
  };`

- Add turbo.json in ui

>turbo.json
`{
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
}`
>package.json (can replace the whole thing just edit the name )
`{
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
`
- Add a empty styles.css file in ui/src
<img width="284" height="229" alt="image" src="https://github.com/user-attachments/assets/1ffad552-01de-47b2-a899-8bc6decc8af4" />


- Run this in root folder (only here we changed the things rest all is similar to original docs.)
  > terminal
  `npm install @repo/ui @repo/tailwind-config`

- Add ./apps/web/app/globals.css (same goes for the docs )
  > globals.css
`@import 'tailwindcss';
@import '@repo/tailwind-config';
@import '@repo/ui/styles.css';`
-  Add postcss.config.js in web
  >postcss.config.js
`import { postcssConfig } from '@repo/tailwind-config/postcss';
 
export default postcssConfig;`

Done !!!!

### Check the code if you need any assist...


  

  


