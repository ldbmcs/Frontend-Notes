# Using ESLint and Prettier in a TypeScript Project

https://robertcooper.me/post/using-eslint-and-prettier-in-a-typescript-project

When it comes to linting TypeScript code, there are two major linting options to choose from: [TSLint](https://palantir.github.io/tslint/) and [ESLint](https://eslint.org/). TSLint is a linter that can only be used for TypeScript, while ESLint supports both JavaScript and TypeScript.

In the [TypeScript 2019 Roadmap](https://github.com/Microsoft/TypeScript/issues/29288#developer-productivity-tools-and-integration), the TypeScript core team explains that **ESLint has a more performant architecture than TSLint** and that they will **only be focusing on ESLint** when providing editor linting integration for TypeScript. For that reason, I would recommend using ESLint for linting TypeScript projects.

## 1. Setting up ESLint to work with TypeScript

First, install all the required dev dependencies:

```bash
yarn add eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin --dev
```

> If using `create-react-app` to bootstrap a project, `eslint` is already included as a dependency through `react-scripts`, and therefore it is not necessary to explicitly install it with `yarn`.

- [`eslint`](https://www.npmjs.com/package/eslint): The core ESLint linting library
- [`@typescript-eslint/parser`](https://www.npmjs.com/package/@typescript-eslint/parser): The parser that will allow ESLint to lint TypeScript code
- [`@typescript-eslint/eslint-plugin`](https://www.npmjs.com/package/@typescript-eslint/eslint-plugin): A plugin that contains a bunch of ESLint rules that are TypeScript specific

Next, add an `.eslintrc.js` configuration file in the root project directory. Here is a sample configuration for a TypeScript project:

```javascript
module.exports = {
  parser: "@typescript-eslint/parser", // Specifies the ESLint parser
  parserOptions: {
    ecmaVersion: 2020, // Allows for the parsing of modern ECMAScript features
    sourceType: "module" // Allows for the use of imports
  },
  extends: [
    "plugin:@typescript-eslint/recommended" // Uses the recommended rules from the @typescript-eslint/eslint-plugin
  ],
  rules: {
    // Place to specify ESLint rules. Can be used to overwrite rules specified from the extended configs
    // e.g. "@typescript-eslint/explicit-function-return-type": "off",
  }
};
```

> I prefer using a JavaScript file for the `.eslintrc` file (instead of a JSON file) as it supports comments that can be used to better describe rules.

If using TypeScript with React, the [`eslint-plugin-react`](https://www.npmjs.com/package/eslint-plugin-react) dev dependency should be installed and the following configuration can be used:

```javascript
module.exports = {
  parser: "@typescript-eslint/parser", // Specifies the ESLint parser
  parserOptions: {
    ecmaVersion: 2020, // Allows for the parsing of modern ECMAScript features
    sourceType: "module", // Allows for the use of imports
    ecmaFeatures: {
      jsx: true // Allows for the parsing of JSX
    }
  },
  settings: {
    react: {
      version: "detect" // Tells eslint-plugin-react to automatically detect the version of React to use
    }
  },
  extends: [
    "plugin:react/recommended", // Uses the recommended rules from @eslint-plugin-react
    "plugin:@typescript-eslint/recommended" // Uses the recommended rules from @typescript-eslint/eslint-plugin
  ],
  rules: {
    // Place to specify ESLint rules. Can be used to overwrite rules specified from the extended configs
    // e.g. "@typescript-eslint/explicit-function-return-type": "off",
  },
};
```

Ultimately it's up to you to decide what rules you would like to extend from and which ones to use within the `rules` object in your `.eslintrc.js` file.

## 2. Adding Prettier to the mix

What works well along with ESLint is [prettier](https://prettier.io/), which does a great job at handling code formatting. Install the required dev dependencies to get prettier working with ESLint:

```bash
yarn add prettier eslint-config-prettier eslint-plugin-prettier --dev
```

- [`prettier`](https://www.npmjs.com/package/prettier): The core prettier library
- [`eslint-config-prettier`](https://www.npmjs.com/package/eslint-config-prettier): Disables ESLint rules that might conflict with prettier
- [`eslint-plugin-prettier`](https://www.npmjs.com/package/eslint-plugin-prettier): Runs prettier as an ESLint rule

In order to configure prettier, a `.prettierrc.js` file is required at the root project directory. Here is a sample `.prettierrc.js` file:

```javascript
module.exports = {
  semi: true,
  trailingComma: "all",
  singleQuote: true,
  printWidth: 120,
  tabWidth: 4
};
```

Next, the `.eslintrc.js` file needs to be updated:

```javascript
module.exports = {
  parser: "@typescript-eslint/parser", // Specifies the ESLint parser
  parserOptions: {
    ecmaVersion: 2020, // Allows for the parsing of modern ECMAScript features
    sourceType: "module", // Allows for the use of imports
    ecmaFeatures: {
      jsx: true // Allows for the parsing of JSX
    }
  },
  settings: {
    react: {
      version: "detect" // Tells eslint-plugin-react to automatically detect the version of React to use
    }
  },
  extends: [
    "plugin:react/recommended", // Uses the recommended rules from @eslint-plugin-react
    "plugin:@typescript-eslint/recommended", // Uses the recommended rules from the @typescript-eslint/eslint-plugin
    "prettier/@typescript-eslint", // Uses eslint-config-prettier to disable ESLint rules from @typescript-eslint/eslint-plugin that would conflict with prettier
    "plugin:prettier/recommended" // Enables eslint-plugin-prettier and eslint-config-prettier. This will display prettier errors as ESLint errors. Make sure this is always the last configuration in the extends array.
  ],
  rules: {
    // Place to specify ESLint rules. Can be used to overwrite rules specified from the extended configs
    // e.g. "@typescript-eslint/explicit-function-return-type": "off",
  },
};
```

> Make sure that `plugin:prettier/recommended` is the last configuration in the `extends` array

The advantage of having prettier setup as an ESLint rule using `eslint-plugin-prettier` is that code can automatically be fixed using ESLint's `--fix` option.

## 3. Automatically Fix Code in VS Code

For a good developer experience, it's useful to setup your editor to automatically run ESLint's automatic fix command (i.e. `eslint --fix`) whenever a file is saved. Since i'm using VS Code, here is the config required in the `settings.json` file in VS Code to get automatic fixing whenever saving a file:

```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
}
```

## 4. Run ESLint with the CLI

A useful command to add to the [`package.json` scripts](https://docs.npmjs.com/misc/scripts) is a `lint` command that will run ESLint.

```json
{
  "scripts": {
    "lint": "eslint '*/**/*.{js,ts,tsx}' --quiet --fix"
  }
}
```

The above script can be run from the command line using `npm run lint` or `yarn lint`. This command will run ESLint through all the `.js`, `.ts`, and `.tsx` (used with React) files. Any ESLint errors that can be automatically fixed will be fixed with this command, but any other errors will be printed out in the command line.

[ESLint CLI Options](https://eslint.org/docs/user-guide/command-line-interface)

Even if ESLint doesn't report any errors, it doesn't necessarily mean that the TypeScript compiler won't report any type errors. To verify that your code has type errors, the `tsc --noEmit` command can used.

## 5. Preventing ESLint and formatting errors from being committed

To ensure all files committed to git don't have any linting or formatting errors, there is a tool called [`lint-staged`](https://github.com/okonet/lint-staged) that can be used. `lint-staged` allows to run linting commands on files that are staged to be committed. When `lint-staged` is used in combination with [`husky`](https://github.com/typicode/husky), the linting commands specified with `lint-staged` can be executed to staged files on pre-commit (if unfamiliar with git hooks, read about them [here](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)).

To configure `lint-staged` and `husky`, add the following configuration to the `package.json` file:

```json
{
  "husky": {
      "hooks": {
          "pre-commit": "lint-staged"
      }
  },
  "lint-staged": {
      "*.{js,ts,tsx}": [
          "eslint --fix"
      ]
  }
}
```

The above configuration will run `lint-staged` when a user tries to commit code to git. `lint-staged` will then run ESLint on any staged files with `.js`, `.ts`, and `.tsx` extensions. Any errors that can be fixed automatically will be fixed and added to the current commit. However, if there are any linting errors that cannot be fixed automatically, the commit will fail and the errors will need to be manually fixed before trying to commit the code again.

Unfortunately it is not enough to only rely on `lint-staged` and `husky` to prevent linting errors since the git hooks can be by-passed if a user commits uses [the `--no-verify` flag](https://git-scm.com/docs/git-commit#Documentation/git-commit.txt---no-verify). Therefore, it is also recommended to run a command on a continuous integration (CI) server that will verify that there are no linting errors. That command should look like the following:

```bash
eslint '*/**/*.{js,ts,tsx}' --quiet
```

Notice the above command doesn't pass the `--fix` command to the `eslint` CLI since we want the command to fail if there are any sort of errors. We do not want the CI automatically fixing lint errors since that would indicate that there is commited code that does not pass the linting checks.

---

That's how you can lint and format a TypeScript project using ESLint and Prettier 😎

