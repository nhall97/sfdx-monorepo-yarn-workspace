# sfdx-monorepo-yarn-workspace
sfdx monorepo using yarn Workspaces to work with multiple sfdx projects eg. packages

This is to provide a monorepo structure to our sfdx packages. The idea being that you can clone one repo, and work across many packages - to help make dependency management easier as well as refactoring.

The goal is to be able to provide a template sfdx-monorepo-workspace - to allow people to follow the same pattern with ease.

Initial implementation was done trying to use npm workspaces - but as of npm v7.20.3 - there seems to be an issue with `npm install` in the Google Cloudbuild environment. https://github.com/nhall97/sfdx-monorepo-npm-workspace 

## Project Structure
The project is structured in the following
```
.yarn
packages/
    /sfdx-standard-project-1
        /package.json
    /sfdx-standard-project-2
        /package.json
yarn.yml
package.json
cloudbuild.yaml
yarn.lock
 ```

## Enable workspaces
```yarn plugin import workspace-tools```

```yarn install```

## Using workspace/monorepo structure and sf cli
### Verify workspace
Verify packages are all working as expected:

```$yarn workspaces foreach -R run verify-sf```

### Deploy Package
Deploy the source files in a directory:

```$sf deploy metadata --source-dir path/to/source```

Deploy demo package:

```$yarn sf deploy metadata --source-dir packages/demo-package```

Deploy foobar package:

```$yarn sf deploy metadata --source-dir packages/foobar-package```

Deploy all packages located in workspace:

```$yarn workspaces foreach -R run deploy```
