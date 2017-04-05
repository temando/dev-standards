# Projects

Best practices authoring projects and services

---

1. [Services](#services)

---

## Services

#### `1.1` Dependencies

- Never rely on global dependencies
- Store everything within your `package.json`

#### ✘ BAD

Imagine a deployment script for a serverless project.

```bash
#!/bin/bash

registry service pull-env -r $T_AWS_REGION $T_ENV
sls deploy

```

This is bad because:
- ✘ You are assuming `sls` and `registry` exist
- ✘ You are assuming they are the right version

#### ✓ GOOD

```bash
#!/bin/bash

yarn registry -- service pull-env -r $T_AWS_REGION $T_ENV
yarn sls -- deploy
```

We're using `yarn` to run the binary from `./node_modules/.bin`, thus the version of `sls` and `registry` are locked down to what we define.