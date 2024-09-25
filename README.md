# GitHub Actions CI/CD for Node.js Application Deployment to Heroku

## Project Overview

This project demonstrates how to set up Continuous Integration (CI) and Continuous Deployment (CD) workflows using **GitHub Actions**. The application is a simple Node.js app deployed to **Heroku** automatically after successful builds.

## Objectives

The objective of this project is to automate the process of building, testing, and deploying a Node.js application using GitHub Actions workflows.

## Features

- **Continuous Integration (CI):** Automatically runs on each push to the `main` branch, installing dependencies, running basic build steps.
- **Continuous Deployment (CD):** Deploys the application to **Heroku** automatically after a successful CI workflow.

## Setup and Configurations

### Prerequisites

- **Node.js**: Ensure Node.js is installed. The project uses Node.js version 20.x.
- **Heroku Account**: A Heroku account is required to deploy the application.

### Repository Structure

```
.github/
  workflows/
    main.yml       # GitHub Actions workflow file for CI/CD
node_modules/      # Project dependencies
index.js           # Node.js application entry point
package.json       # Project metadata and dependencies
package-lock.json  # Locked dependency versions
Procfile           # Specifies the command to run the app in Heroku
README.md          # Documentation file
.gitignore         # Ignores unnecessary files in Git version control
```

### CI Workflow (Continuous Integration)

The CI workflow file `.github/workflows/main.yml` defines the steps for automatically building the Node.js project on every push to the `main` branch. The workflow includes:

- **Checkout Code:** Downloads the repository's code.
- **Install Dependencies:** Installs the Node.js dependencies using `npm install`.
- **Build Step:** Runs a basic build command to ensure the project can be compiled.
- **Optional:** You can also add testing steps to automatically run your tests.

```yaml
name: CI/CD Workflow

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Install Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '20.x'

      - name: Install dependencies
        run: npm install

      - name: Build
        run: npm run build
```

### CD Workflow (Continuous Deployment)

After the successful build, the workflow automatically deploys the application to **Heroku** using the **Heroku CLI**.

```yaml
      - name: Install Heroku CLI
        run: |
          curl https://cli-assets.heroku.com/install.sh | sh

      - name: Authenticate Heroku
        run: echo "$HEROKU_API_KEY" | heroku auth:token

      - name: Deploy to Heroku
        run: |
          git remote add heroku https://git.heroku.com/YOUR_APP_NAME.git
          git push heroku main
```

### Heroku Deployment

The application is deployed to Heroku using the following setup:

- **Procfile:** A file in the root of the project that specifies the command used to run the application on Heroku.

```bash
web: npm start
```

- **Index.js:** The entry point for the Node.js application, which runs a simple Express server.

```javascript
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send('Hello, Heroku!');
});

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

### Testing and Validation

To verify that the workflows are working properly:

1. **CI Workflow**: Ensure the build step runs successfully when code is pushed to the `main` branch.
2. **CD Workflow**: Validate that the app deploys successfully to Heroku and can be accessed via the Heroku URL.
   - Access the deployed app at: `https://YOUR_APP_NAME.herokuapp.com`
   - my app currently runs on '[githubactions-aec20e1fb4c7.herokuapp.com] but that could change!


### Example Output

Once deployed, visiting the Heroku app URL will display a message:
```
Hello, Heroku!
```

## Screenshots

Here are some screenshots illustrating the key stages of the GitHub Actions setup:

1. **GitHub Actions Workflow Execution:**

![GitHub Actions](screenshot-github-actions.png)

2. **Successful Heroku Deployment:**

![Heroku Deployment](screenshot-heroku-deployment.png)

## Conclusion

By setting up GitHub Actions for CI/CD, we've automated the testing, building, and deployment process for this Node.js application. This setup ensures that each change to the codebase is automatically tested and deployed, improving development efficiency.

## References

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Heroku CLI Documentation](https://devcenter.heroku.com/articles/heroku-cli)

---

### Action Items:

1. Replace placeholders like `YOUR_APP_NAME` with your actual app details.
2. Add any screenshots or other relevant sections.
3. Commit this `README.md` to your repository.
