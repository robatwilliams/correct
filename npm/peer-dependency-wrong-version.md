## Problem

1. Dependency requires peer matching a specific version range
1. Peer is installed but it doesn't match that range

## Consequence

Application unknowingly uses incompatible dependencies, which happen to work together fine most of the time.

## Cause 1

`npm install` logs `WARN` rather than `ERROR` for peer dependency issues. Likely reason for this is to allow you to use supposedly incompatible versions if you know what you're doing, thus avoiding dependency hell.

### Solution

Add a `postinstall` NPM [list](https://docs.npmjs.com/cli/ls) script which will error on peer dependency issues:

```json
{
  "scripts": {
    "postinstall": "npm ls --depth=0"
  }
}
```

## Cause 2

When running in Git Bash on Windows, NPM [fails to detect](https://stackoverflow.com/questions/32742865/no-console-colors-if-using-npm-script-inside-a-git-bash-mintty) that the shell supports colours, so the `WARN` doesn't have a yellow background, making warnings less noticeable.

### Solution

Configure NPM to [always show colours](https://docs.npmjs.com/misc/config#color), via user or project-local `.npmrc` file:

```ini
color = always
```

## Cause 3

Output of `npm install` polluted by other warnings due to missing `package.json` fields, making important warnings less noticeable.

### Solution

Populate the `description`, `repository`, and `license` fields. For non-open-source projects, you [can use](https://docs.npmjs.com/files/package.json#license) `UNLICENSED`.

Alternatively, mark the package as `private: true`. This will also make NPM refuse to publish the package.
