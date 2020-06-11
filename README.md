![build-test](https://github.com/jwgmeligmeyling/spotbugs-github-action/workflows/build-test/badge.svg)

# SpotBugs GitHub Action

This action pushes results from [SpotBugs](https://spotbugs.github.io/) (or [FindBugs](http://findbugs.sourceforge.net/)) as check run annotations. :rocket:

The action can also be used for any other static analysis tools that produce reports in the SpotBugs XML format.
The report itself must be generated in a former build step, for example a Maven build.

![example](images/example.png)

## Input

### `path`
Required. A file, directory or wildcard pattern that describes where to find the reports.
Multiple files can be processed through a [glob expression](https://github.com/actions/toolkit/tree/master/packages/glob), for example: `'**/spotbugsXml.xml'`.

### `name`
Optional. Name for the check run to create. Defaults to `spotbugs`.

### `title`
Optional. Title for the check run to create. Defaults to `SpotBugs Source Code Analyzer report`.

### `token`
Optional. GitHub API access token. Defaults to `${{ github.token }}`, which is set by `actions/checkout@v2` minimally.

## Example usage

```yaml
name: Java CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    - uses: actions/cache@v1
      with:
        path: ~/.m2/repository
        key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
        restore-keys: |
          ${{ runner.os }}-maven-
    - name: Build with Maven
      run: mvn -B verify spotbugs:spotbugs
    - uses: jwgmeligmeyling/spotbugs-github-action@v1
      with:
        path: '**/spotbugsXml.xml'
```

And do not forget to enable XML output for the Maven plugin:

```xml
<build>
  <plugins>
    <plugin>
      <groupId>com.github.spotbugs</groupId>
      <artifactId>spotbugs-maven-plugin</artifactId>
      <version>4.0.0</version>
      <configuration>
        <xmlOutput>true</xmlOutput>
        <failOnError>false</failOnError>
      </configuration>
    </plugin>
  </plugins>
</build>
```

## Contributing

Install the dependencies  
```bash
$ npm install
```

Build the typescript and package it for distribution
```bash
$ npm run build && npm run pack
```

Run the tests :heavy_check_mark:  
```bash
$ npm test

 PASS  ./index.test.js
  ✓ throws invalid number (3ms)
  ✓ wait 500 ms (504ms)
  ✓ test runs (95ms)

...
```
