# Baseline-Buddy

Baseline Buddy helps web developers adopt modern web features safely by integrating Baseline data (web-features npm + Web Platform Dashboard) into developer tools: an ESLint plugin, a CLI analyzer, a VS Code extension, and a lightweight hosted dashboard.

 > NOTE: This repository is a monorepo. Each package lives under `packages/`. See the "Monorepo layout" section for details.
---

## Table of Contents

- [Key features](#key-features)
- [Monorepo layout](#monorepo-layout)
- [Getting started (development)](#getting-started-development)
- [Quickstart examples](#quickstart-examples)
- [ESLint (project integration)](#eslint-project-integration)
- [VS Code extension (dev)](#vs-code-extension-dev)
- [Dashboard (local dev)](#dashboard-local-dev)
- [CI / Tests](#ci--tests)
- [Packaging and publishing](#packaging-and-publishing)
- [Contributing](#contributing)
- [License](#license)
- [Demo / Video / Hosted demo](#demo--video--hosted-demo)
- [Judging / Testing instructions (for hackaton judges)](#judging--testing-instructions-for-hackaton-judges)

---

## Key features

- Syncs Baseline compatibility data from `web-features` npm + Web Platform Dashboard.
- ESLint plugin that flags features with partial/no support on configured targets and suggests fixes or polyfills.
- CLI analyzer (`baseline-buddy analyze`) to generate per-project compatibility reports.
- VS Code extension with inline hovers and quick-fixes.
- Hosted dashboard to visualize flagged features, browser support heatmap and per-file drilldowns.
- Opt-in analytics for aggregated trends (fully opt-in).

---

## Monorepo layout

baseline-buddy/
├─ packages/
│ ├─ web-features-sync/ # sync CLI + normalized DB
│ ├─ eslint-plugin-baseline/ # ESLint plugin
│ ├─ baseline-cli/ # CLI analyzer
│ ├─ vscode-baseline/ # VS Code extension (dev)
│ └─ sample-project/ # small sample repo for tests
├─ dashboard/ # React app + serverless functions
├─ data/ # generated baseline DB (gitignored; sample included)
├─ README.md
├─ LICENSE.txt
├─ package.json # workspace root
└─ .github/workflows/ci.yml

## Getting started (development)

Prerequisites:

- Node.js 18+ (Node 20 recommended).
- npm 8+ or pnpm/yarn (this README uses npm examples).
- VS Code (for extension development).
- Git.

1. Clone the repository:

   ```bash
   git clone https://github.com/Twinkym/baseline-buddy.git
   cd baseline-buddy 
   ```

2. Install dependencies (root workspace):

   ```bash
   npm install
   ```

3. Sync baseline data (creates data/baseline.json):

   ```bash
   npx baseline-buddy sync
   # or from package:
   node packages/web-features-sync/dist/index.js
   ```

4. Build packages (TypeScript):

   ```bash
   npm run build
   ```

## Quickstart examples

   ```bash
   npx baseline-buddy analyze ./packages/sample-project/src --target "last 2 versions, not dead"
   # Output: human-friendly summary + writes reports/report.json
   ```

## ESLint (project integration)

1. Install plugin locally in target project:

   ```bash
   npm install --save-dev eslint-plugin-baseline
   ```

2. .eslintrc.cjs example:

   ```js

   module.exports = {
     plugins: ["baseline"],
     rules: {
       "baseline/no-unsupported": ["warn", { target: "last 2 versions, not dead"}]
     }
   };
   ```

## VS Code extension (dev)

 · Run the extension host from packages/vscode-baseline with F5 to test extension locally.
 · Hovers and quick-fixes will show for flagged features with links to MDN and suggested polyfills.

## Dashboard (local dev)

1. Move to the dashboard folder:

   ```bash

   cd dashboard
   npm install
   npm run dev
   ```

2. The dashboard reads data/baseline.json and local report artifacts(e.g: reports/report.json) for visualizations

## CI / Tests

· Unit test: each package uses Jest (or vitest).
· Integration test: baseline-buddy analyze packages/sample.project/src -> assert know flags.
· GitHub Actions workflow runs npm ci && npm build && npm test.

## Packaging and publishing

· Each package is ready to be published to npm; packages/*/package.json contains name, version, main fields.
· Use npm publish --access public from each package folder (or use a release pipeline).

## Contributing

Contributions welcome! Please:
  · Fork the repo and create a branch for your feature or bugfix.
  · Write tests for new functionality.
  · Open a pull request with a clear description.

## License

This project is licensed under the MIT License. See LICENSE.txt for details.

## Demo / Video / Hosted demo

· Repo: <https://github.com/Twinkym/baseline-buddy>
· Hosted demo: "Colocar la URL una vez hosteado el proyecto".
· Demo video (Youtube/Vimeo):<https://youtube.com/>videoid "Colocar la id del video una vez subido la video demostración".

## Judging / Testing instructions (for hackaton judges)

1. Clone:

   ```bash

   git clone https://github.com/Twinkym/baseline.buddy.git
   cd baseline-buddy
   ```

2. Install:

   ```bash

   npm install
   ```

3. Sync baseline data (creates data/baseline.json):

   ```bash

   npx baseline-buddy sync
   ```

4. Analyze the sample project:

   ```bash

   npx baseline-buddy analyze ./packages/sample-project/src --target "last 2 versions, not dead".
   # Check generated report in reports/report.json`
   ```

5. Try ESLint:

   ```bash

   cd packages/sample-project
   npm install
   npx eslint src --ext .js, .ts
   ```

6. Visit the hosted dashboard (if available) to inspect heatmaps and drilldowns.
