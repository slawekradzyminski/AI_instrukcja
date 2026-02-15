# Which command installs Playwright browser binaries after adding the package?

## Theory

## Installing Playwright

Setting up Playwright is straightforward, but the installation process differs slightly depending on your preferred package manager and programming language.

### For JavaScript/TypeScript Projects

**Using npm:**
```bash
npm init playwright@latest
# or for existing projects
npm install -D @playwright/test
npx playwright install
```

**Using yarn:**
```bash
yarn create playwright
# or for existing projects  
yarn add -D @playwright/test
npx playwright install
```

**Using pnpm:**
```bash
pnpm create playwright
# or for existing projects
pnpm add -D @playwright/test
npx playwright install
```

### What Gets Installed?

1. **@playwright/test**: The testing framework and runner
2. **Browser binaries**: Chromium, Firefox, and WebKit browsers
3. **System dependencies**: Required libraries for browser operation

### The Browser Installation Step

The `npx playwright install` command downloads browser binaries (~200MB each):
```bash
npx playwright install          # All browsers
npx playwright install chromium # Specific browser only
npx playwright install --with-deps # Include system dependencies
```

### Verification

After installation, verify with:
```bash
npx playwright test --help
npx playwright --version
```

## Question

Which command installs Playwright browser binaries after adding the package?

## Options

- **A.** npm install playwright
- **B.** playwright setup
- **C.** npx playwright install
- **D.** npx playwright init

## Correct Answer

**C**

## Explanation

## Why Option C is Correct

**Option C** `npx playwright install` is the correct command to install browser binaries.

### Understanding the Installation Process

**Step 1**: Install the Playwright package
```bash
npm install -D @playwright/test
```

**Step 2**: Install browser binaries (this is what the question asks about)
```bash
npx playwright install  # ✅ Correct answer
```

### What This Command Does:
- Downloads Chromium, Firefox, and WebKit browsers
- Installs them in a Playwright-managed directory
- Ensures consistent browser versions across environments
- Downloads ~200MB per browser

### Analysis of Other Options:

**❌ Option A** `npm install playwright`:
```bash
# This only installs the package, not browsers
npm install playwright
# Still need: npx playwright install
```

**❌ Option B** `playwright setup`:
```bash
# This command doesn't exist
playwright setup  # ❌ Invalid command
```

**❌ Option D** `npx playwright init`:
```bash
# This initializes a new project, doesn't install browsers
npx playwright init
```

### 💡 Pro Tip:
For CI/CD environments, use:
```bash
npx playwright install --with-deps
```
This installs system dependencies needed for browsers on Linux.
