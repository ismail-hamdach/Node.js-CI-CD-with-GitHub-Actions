
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
```

### Release Workflow

```yaml
name: Release

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - run: docker build . -t ismail-hamdach/node-app
      - run: echo "${{secrets.DOCKER_PASSWORD}}" | docker login -u ${{secrets.DOCKERHUB_USERNAME}} --password-stdin
      - run: docker push ismail-hamdach/node-app
```

## Getting Started

1. **Clone the Repository**:
    ```bash
    git clone https://github.com/ismail-hamdach/node-github-actions-demo.git
    cd node-github-actions-demo
    ```

2. **Install Dependencies**:
    ```bash
    npm install
    ```

3. **Run the Application**:
    ```bash
    npm start
    ```

4. **Run Tests**:
    ```bash
    npm test
    ```

## Contributing

Contributions are welcome! Please feel free to submit issues, fork the repository, and send pull requests.

## License

This project is licensed under the MIT License.
