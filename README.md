# docusaurus-yarn-v2-example

## Rationale

Docusaurus v2 doesn't work with Yarn v2 in pnp mode out of the box.

This repo shows you what (hacks) need to be done.

It also shows what needs to be properly declared as dependencies in docusaurus v2.

## Steps and explanations

### Step 1: Initialize Docusaurus v2

See [official docs](https://v2.docusaurus.io/docs/installation)

### Step 2: Initialize Yarn v2

```bash
yarn set version berry
yarn install
```

### Step 3: Use `pnpMode: loose`

Append line `pnpMode: loose` to `.yarnrc.yml`.

Reason: Docusaurus relies on a lot of packages that doesn't properly declare their dependencies.
Using the default strict mode isn't possible.

### Step 4: Add a bunch of `devDependencies`

```bash
yarn add --dev "@babel/plugin-syntax-dynamic-import" "@babel/plugin-transform-runtime" "@babel/preset-env" "@babel/preset-react" "@babel/preset-typescript" "babel-plugin-dynamic-import-node"
```

PNP has a fallback that all dependencies declared in your `package.json` will act as fallback.
When package `a` requires `b`, but `a` doesn't declare `b` as dependency, it will fail unless `b` is declared
as a dependency in user's `package.json`.

To fix them, docusaurus needs to use `require.resolve` to load these presets and plugins.

### Step 5: Add `"@yarnpkg/pnpify"`

```bash
yarn add --dev "@yarnpkg/pnpify"
```

At this point, there is still some broken dependencies that tried to do some crazy stuff.
I don't spend time investigating what they are doing, but using `pnpify` that simulates an in-memory `node_modules` helps
to solve the issue.

### Step 6: Append `pnpify` to all scripts

This step actually uses `pnpify`.

### Step 7: Profit

Now `yarn start` and `yarn build` can work.
