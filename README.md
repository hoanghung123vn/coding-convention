### Setup coding convention

- Add dev dependency

```
yarn add --dev husky @commitlint/{config-conventional,cli} lint-staged eslint prettier
```

- Setup
  Add ESlint config `.eslintrc.js`

```js
"rules": {
    "@typescript-eslint/interface-name-prefix": "off",
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/explicit-module-boundary-types": "off",
    "@typescript-eslint/no-explicit-any": "off",
    "semi": ["error", "always"], // auto add semi colon
    "no-unused-vars": ["error", { "vars": "all" }] // check var unused
}
```

Add config to your `settings.json` in VScode

```json
"editor.codeActionsOnSave": {
  "source.fixAll.tslint": true,
  "source.fixAll.eslint": true
},
```

Setup husky

```bash
npx husky install
npx husky add .husky/pre-commit "yarn lint-staged"
npx husky add .husky/commit-msg ""
```

Change `.husky/commit-msg`

```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install commitlint --edit "$1"
```

Add file `.commitlintrc.js`

```
module.exports = {extends: ['@commitlint/config-conventional']};
```

Add to your package.json

```
"scripts": {
    ...
    "postinstall": "husky install",
    "lint-staged": "lint-staged",
    "lint": "eslint \"{src,apps,libs,test}/**/*.{tsx,ts}\" --fix",
    "format": "prettier \"src/**/*.{tsx,ts}\" --write"
  },
  "lint-staged": {
    "*.{tsx,ts}": [
      "eslint --fix",
      "prettier --write"
    ]
  },
```

## Code commit rules:

#### Format: type(scope?): subject

#### Type:

- build: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
- ci: Changes to our CI configuration files and scripts (example scopes: Gitlab CI, Circle, BrowserStack, SauceLabs)
- chore: add something without touching production code (Eg: update npm dependencies)
- docs: Documentation only changes
- feat: A new feature
- fix: A bug fix
- perf: A code change that improves performance
- refactor: A code change that neither fixes a bug nor adds a feature
- revert: Reverts a previous commit
- style: Changes that do not affect the meaning of the code (Eg: adding white-space, formatting, missing semi-colons, etc)
- test: Adding missing tests or correcting existing tests

#### Scope: Package/app/lib name

#### Subject: Commit content
