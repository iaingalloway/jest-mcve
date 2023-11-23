# Jest MCVE

An MVCE for an issue with weird jest path matching behaviour.

## Usage

1. Clone this repository
2. Open the repository in VS Code
3. When prompted "Folder contains a Dev Container configuration file. Reopen folder to develop in a container." click "Reopen in Container" (or press `Ctrl+Shift+P` and select "Dev Containers: Reopen in Container")
4. Open the integrated terminal and follow along with the following commands:

```bash
> pwd
/workspaces/jest-mvce

> npx jest --testMatch 'src/tests/hello-world.test.js'
No tests found, exiting with code 1
Run with `--passWithNoTests` to exit with code 0
In /workspaces/jest-mvce
  4 files checked.
  testMatch: src/tests/hello-world.test.js - 0 matches
  testPathIgnorePatterns: /node_modules/ - 4 matches
  testRegex:  - 0 matches
Pattern:  - 0 matches

> npx jest --testMatch './src/tests/hello-world.test.js'
No tests found, exiting with code 1
Run with `--passWithNoTests` to exit with code 0
In /workspaces/jest-mvce
  4 files checked.
  testMatch: ./src/tests/hello-world.test.js - 0 matches
  testPathIgnorePatterns: /node_modules/ - 4 matches
  testRegex:  - 0 matches
Pattern:  - 0 matches

> npx jest --testMatch '/workspaces/jest-mvce/src/tests/hello-world.test.js'
 PASS  src/tests/hello-world.test.js
  ✓ adds 1 + 2 to equal 3 (1 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.197 s
Ran all test suites.

> npx jest --testMatch 'src/**/*.test.js'
No tests found, exiting with code 1
Run with `--passWithNoTests` to exit with code 0
In /workspaces/jest-mvce
  4 files checked.
  testMatch: src/**/*.test.js - 0 matches
  testPathIgnorePatterns: /node_modules/ - 4 matches
  testRegex:  - 0 matches
Pattern:  - 0 matches

> npx jest --testMatch '/workspaces/jest-mvce/src/**/*.test.js'
 PASS  src/tests/hello-world.test.js
  ✓ adds 1 + 2 to equal 3 (1 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.186 s, estimated 1 s
Ran all test suites.

> npx jest --testMatch '**/*.test.js' --rootDir 'src'
 PASS  src/tests/hello-world.test.js
  ✓ adds 1 + 2 to equal 3 (1 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.189 s
Ran all test suites.
```

## Conclusion

Jest's pattern matching behaviour is a bit wild, but it works if you make sure to use `--rootDir` and glob at the start of `--testMatch`.

Is it because the default rootDir is `/` if it isn't set from package.json? That doesn't seem to be documented [here](https://jestjs.io/docs/configuration#rootdir-string), but it bears investigating.
