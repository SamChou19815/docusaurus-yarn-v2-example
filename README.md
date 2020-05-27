# docusaurus-yarn-v2-example

## Rationale

Docusaurus v2 doesn't work with Yarn v2 in pnp strict mode out of the box.

This repo shows you what (hacks) need to be done.

Note: More hacks needs to be done before v2-alpha-55. If you are using anything older than
v2-alpha-56, read 
[the old guide](https://github.com/SamChou19815/docusaurus-yarn-v2-example/tree/docusaurus-v2-alpha-55).

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

### Step 4: Add `@mdx-js/react`

```bash
yarn add @mdx-js/react
```

There are some client code that imports `@mdx-js/react`. Unfortunately, it cannot be done on the
docusaurus side to support it. You have to add the dependency by yourself.

### Step 5: Profit

Now `yarn start` and `yarn build` can work.
