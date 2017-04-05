# Node JS

NodeJS specific guidence and convention.

---

1. [Version Management](#version-management)
2. [Debugging](#debugging)
3. [CICD](#CICD)

---

## Version Management

#### `1.1` Use `nvm` to manage node versions

`nvm` - Node Version Manager

This allows you to switch between versions of node on the fly.

- [**Install**](https://github.com/creationix/nvm#installation)

#### ✓ GOOD

```sh
nvm install 6
nvm alias default 6
```

#### `1.2` Use `avn` to conform to `.node-version`

A `.node-version` file is the convention for declaring a projects intended node runtime.
This is read by the module `avn`, which interfaces with `nvm` to switch to the projects node version

- **Install**: `npm i -g avn`

#### ✓ GOOD

```sh
cd my-new-project
echo "6" > ".node-version"
cd ~
cd my-new-project
$ avn activated 6 (avn-nvm v6.9.1)
```

## Debugging

TODO:
  - describe vscode examples, links
  - when to use debugging
  - using in serverless projects

## CICD

CICD should be as reproducable as possible.