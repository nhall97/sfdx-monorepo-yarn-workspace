# sfdx-monorepo-workspace
sfdx monorepo using yarn Workspaces to work with multiple sfdx projects eg. packages

This is to provide a monorepo structure to our sfdx packages. The idea being that you can clone one repo, and work across many packages - to help make dependency management easier as well as refactoring.

The goal is to be able to provide a template sfdx-monorepo-workspace - to allow people to follow the same pattern with ease.

Initial implementation was done trying to use npm workspaces - but as of npm v7.20.3 - there seems to be an issue with `npm install` in the Google Cloudbuild environment. https://github.com/nhall97/sfdx-monorepo-npm-workspace 

## Project Structure
The project is structured in the following
```
.yarn
packages/
    /sfdx-core-package
        /package.json
    /sfdx-bonus-package
        /package.json
yarn.yml
package.json
cloudbuild.yaml
yarn.lock
 ```

## Instructions
`git clone <repo>`
`cd sfdx-monorepo-yarn-workspace`
`yarn install`

## Using yarn workspaces
You can use yarn workspaces to manage multiple packages at the same time. https://yarnpkg.com/cli/workspaces/foreach

#### List installed workspaces
`$ yarn workspaces list`

Output:
```
➤ YN0000: .
➤ YN0000: packages/sfdx-bonus-package
➤ YN0000: packages/sfdx-core-package
➤ YN0000: Done in 0s 4ms
```

#### Run all tests for every package (in our case, sfdx-core-package & sfdx-bonus-package)

`yarn workspaces foreach run test`

Output:
```
Already have image: node
➤ YN0000: 
➤ YN0000: > sfdx-bonus-package@1.0.0 test:unit
➤ YN0000: > sfdx-lwc-jest
➤ YN0000: 
➤ YN0000: No tests found, exiting with code 1
➤ YN0000: Run with `--passWithNoTests` to exit with code 0
➤ YN0000: In /workspace/packages/sfdx-bonus-package
➤ YN0000:   1 file checked.
➤ YN0000:   testMatch: **/__tests__/**/*.[jt]s?(x), **/?(*.)+(spec|test).[tj]s?(x) - 0 matches
➤ YN0000:   testPathIgnorePatterns: /workspace/packages/sfdx-bonus-package/node_modules/, /workspace/packages/sfdx-bonus-package/test/specs/ - 1 match
➤ YN0000:   testRegex:  - 0 matches
➤ YN0000: Pattern:  - 0 matches
➤ YN0000: 
➤ YN0000: > sfdx-core-package@1.0.0 test:unit
➤ YN0000: > sfdx-lwc-jest
➤ YN0000: 
➤ YN0000: No tests found, exiting with code 1
➤ YN0000: Run with `--passWithNoTests` to exit with code 0
➤ YN0000: In /workspace/packages/sfdx-core-package
➤ YN0000:   1 file checked.
➤ YN0000:   testMatch: **/__tests__/**/*.[jt]s?(x), **/?(*.)+(spec|test).[tj]s?(x) - 0 matches
➤ YN0000:   testPathIgnorePatterns: /workspace/packages/sfdx-core-package/node_modules/, /workspace/packages/sfdx-core-package/test/specs/ - 1 match
➤ YN0000:   testRegex:  - 0 matches
➤ YN0000: Pattern:  - 0 matches
➤ YN0000: Done in 4s 799ms
```

## SFDX Command Validations
| SFDX Command                                                                                                               | Workspace Command            | Working? |
|----------------------------------------------------------------------------------------------------------------------------|------------------------------|----------|
| "lint": "npm run lint:lwc && npm run lint:aura",                                                                           | npm run lint --ws            |         |
| "lint:aura": "eslint **/aura/**",                                                                                          |                              |          |
| "lint:lwc": "eslint **/lwc/**",                                                                                            |                              |          |
| "test": "npm run test:unit",                                                                                               |                              |          |
| "test:unit": "sfdx-lwc-jest",                                                                                              |                              |          |
| "test:unit:watch": "sfdx-lwc-jest --watch",                                                                                |                              |          |
| "test:unit:debug": "sfdx-lwc-jest --debug",                                                                                |                              |          |
| "test:unit:coverage": "sfdx-lwc-jest --coverage",                                                                          |                              |          |
| "prettier": "prettier --write \"**/*.{cls,cmp,component,css,html,js,json,md,page,trigger,xml,yaml,yml}\"",                 | npm run prettier --ws        |         |
| "prettier:verify": "prettier --list-different \"**/*.{cls,cmp,component,css,html,js,json,md,page,trigger,xml,yaml,yml}\"", | npm run prettier:verify --ws |         |
| "precommit": "lint-staged",                                                                                                | npm run precommit --ws       |         |
| "prepare": "cd ../../ && husky install front/.husky"                                                                       | npm run prepare --ws         |     N    |
