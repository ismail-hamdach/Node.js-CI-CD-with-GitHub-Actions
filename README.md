# Node.js CI/CD with GitHub Actions

This repository demonstrates the setup and configuration of CI/CD pipelines for a Node.js application using GitHub Actions. The workflows are designed to build, test, and deploy the application, ensuring compatibility, reliability, and seamless updates.

## Workflows

### Integration

This workflow triggers on every push and pull request to the `main` branch. It includes the following jobs:

- **Build**: Builds the Node.js application.
- **Unit Tests**: Runs unit tests to verify the functionality of the application.

### Release

This workflow triggers on every push to the `main` branch. It includes the following job:

- **Deploy**: Builds a Docker image and pushes it to Docker Hub.

### Node Versions

The workflows run on the following Node.js versions:
- 12.x
- 14.x
- 16.x

## Workflow Configuration

### Integration Workflow

```yaml
name: Integration

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12.x, 14.x, 16.x]

    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v2
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      - run: npm i
      - run: npm run build

  unit-tests:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [12.x, 14.x, 16.x]

    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v2
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      - run: npm i
      - run: npm run test
