# Release Process

## 1. Create a changeset

After making changes, describe what changed:

```bash
pnpm changeset
```

Follow the prompts:
- Select which packages were affected
- Choose the version bump type (patch / minor / major)
- Write a short summary of the changes

This creates a markdown file in `.changeset/` — commit it along with your code changes.

## 2. Version the packages

```bash
pnpm changeset version
```

This consumes the changeset files, updates `package.json` versions and `CHANGELOG.md` for each affected package. Review the changes and commit them.

## 3. Build and publish

```bash
pnpm publish-all
```

This runs `turbo build` (which compiles TypeScript and copies `LICENSE.md` into each package) and then `changeset publish` to push to npm.

**Note:** Publishing order is handled automatically. Packages that are already at the published version on npm will be skipped.

## Auth

- **Local:** Make sure you're logged in with `npm login` and have a granular access token configured that bypasses 2FA. Set it with:

  ```bash
  npm config set //registry.npmjs.org/:_authToken=npm_YOUR_TOKEN
  ```

- **CI:** Set `NPM_TOKEN` as a secret in your GitHub repository settings and uncomment the auth line in `.npmrc`:

  ```
  //registry.npmjs.org/:_authToken=${NPM_TOKEN}
  ```

## Adding a new provider

When you add a new provider package, remember to also add it as a dependency in `packages/openiac/package.json` so it's included in `@openiac/all`.
