# Which command installs dependencies exactly from package-lock.json?

## Theory

## npm, npx and package.json

- `package.json` lists the project's dependencies and scripts.
- `package-lock.json` pins the exact versions that were tested.

```bash
npm ci                      # clean install exactly from package-lock.json
npm install                 # installs and may update package-lock.json
npx playwright test         # runs a tool installed in the project
npm run test:api            # runs the "test:api" script from package.json
```

After cloning a training repository, use `npm ci` so everyone has the same versions.

## Question

After cloning a project, which command installs dependencies exactly as recorded in `package-lock.json`?

## Options

- **A.** `npx install`
- **B.** `npm start`
- **C.** `npm run build`
- **D.** `npm ci`

## Correct Answer

**D**

## Explanation

**Option D** is correct. `npm ci` (clean install) removes `node_modules` and installs exactly the versions from `package-lock.json`. It fails if `package.json` and the lock file disagree, which keeps environments reproducible.

- **A** is not a standard command; `npx` runs tools, it does not install project dependencies.
- **B** runs the `start` script, if one exists.
- **C** runs the `build` script.

`npm install` also installs dependencies, but it may update the lock file, so `npm ci` is preferred for a fresh checkout.
