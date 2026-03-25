# White-Label Mintlify Docs Plan

## Recommendation

The best option is **one Git repo with one shared content source and three brand-specific Mintlify deployments**.

Do **not** maintain three manual clones of this repo unless each merchant will quickly become a different product with different docs ownership.

For this repo, most of the content appears reusable and the merchant-specific changes will likely be:

- brand name
- logo and favicon
- colors
- support email / dashboard / website links
- custom domain
- a small number of merchant-specific pages or overrides

That makes a **shared-content + per-merchant config** model the lowest-maintenance setup.

## Why this is the best fit

This repo is currently a standard Mintlify site with:

- one global [`docs.json`](/Users/michealakinwonmi/workspace/payfonte/services/mintlify-docs/docs.json)
- shared MDX under [`guides/`](/Users/michealakinwonmi/workspace/payfonte/services/mintlify-docs/guides)
- shared OpenAPI under [`api-reference/openapi-3.1.yml`](/Users/michealakinwonmi/workspace/payfonte/services/mintlify-docs/api-reference/openapi-3.1.yml)

In practice, white-label docs usually need different branding and links, which means each merchant needs its **own** `docs.json`.

The cleanest way to do that is:

1. Keep the docs content shared in one repo.
2. Create one deployable Mintlify site folder per merchant.
3. Point each Mintlify project to its own folder in the same repo.
4. Attach a different custom domain to each project.

This gives you:

- one place to update core docs
- separate branding per merchant
- separate deployment targets
- lower risk of content drift
- simpler review and versioning

## Recommended repo shape

```text
mintlify-docs/
  shared/
    guides/
    api-reference/
    images/
  merchants/
    merchant-a/
      docs.json
      logo/
      favicon/
      overrides/
    merchant-b/
      docs.json
      logo/
      favicon/
      overrides/
    merchant-c/
      docs.json
      logo/
      favicon/
      overrides/
  deployments/
    merchant-a/
    merchant-b/
    merchant-c/
  scripts/
    build-merchant-docs.js
```

## How it should work

Use `shared/` as the single source of truth for common docs.

For each merchant:

- store branding and links in `merchants/<merchant>/docs.json`
- store merchant-only pages in `merchants/<merchant>/overrides/`
- generate a deployable site into `deployments/<merchant>/`

The generation step should:

1. Copy shared docs into `deployments/<merchant>/`
2. Copy the merchant `docs.json`
3. Copy merchant logos / favicons
4. Apply any page overrides
5. Optionally swap merchant-specific values in shared pages if needed

## Deployment model

Create **three Mintlify projects** in the dashboard:

- Merchant A -> repo path `deployments/merchant-a`
- Merchant B -> repo path `deployments/merchant-b`
- Merchant C -> repo path `deployments/merchant-c`

Then connect each project to its own custom domain.

This is better than separate repos because it keeps shared docs changes synchronized by default.

## What not to do

Avoid these unless there is a strong reason:

- **Three separate repos**: high maintenance, duplicated fixes, easy content drift.
- **Three long-lived branches**: hard to merge, poor visibility, operationally fragile.
- **One `docs.json` with runtime switching**: not a good fit because Mintlify site config is global per deployment.

## When separate repos are justified

Move to separate repos only if one or more of these become true:

- each merchant has a different product surface
- content divergence grows beyond about 20-30%
- merchants need different release cadences
- different teams own the docs independently
- compliance requires stronger isolation

If that happens, start from the shared-content model first, then split later only where the cost is justified.

## Rollout plan

### Phase 1

Restructure this repo into shared content plus merchant configs.

Output:

- `shared/`
- `merchants/merchant-a`
- `merchants/merchant-b`
- `merchants/merchant-c`
- `scripts/build-merchant-docs.js`

### Phase 2

Implement a generator script that builds each deployable Mintlify folder.

Output:

- `deployments/merchant-a`
- `deployments/merchant-b`
- `deployments/merchant-c`

### Phase 3

Set up three Mintlify projects, each pointing to its deployment folder.

Output:

- three independent branded doc sites
- one shared authoring workflow

### Phase 4

Add CI to regenerate deployment folders on every change and fail if generated output is stale.

Output:

- predictable deployments
- reduced manual errors

## Practical rules

- Keep shared content shared by default.
- Use overrides only when a merchant really differs.
- Treat `docs.json` as merchant-owned configuration.
- Treat OpenAPI as shared unless a merchant truly has a different API surface.
- Review divergence every quarter; split only if the shared model stops paying off.

## Bottom line

For three white-labeled merchants, the best setup is **one repo, three Mintlify projects, shared docs content, and one build step that produces one deployable folder per merchant**.

That gives you the branding separation you need without paying the maintenance cost of three full clones.
