# Class Intro - React App

A React application for class introduction and tutorials, built with [Create React App](https://github.com/facebook/create-react-app).

## Table of Contents

- [Prerequisites](#prerequisites)
- [Dependencies](#dependencies)
- [Installation](#installation)
- [Running the App](#running-the-app)
- [Available Scripts](#available-scripts)
- [Docker Configuration](#docker-configuration)
- [Testing](#testing)
- [Building for Production](#building-for-production)

## Prerequisites

Before you begin, ensure you have the following installed:

### Option 1: With Docker (Recommended)

- **Docker Desktop**: [Download Docker Desktop](https://www.docker.com/products/docker-desktop) (includes Docker Compose)
- **Git**: For cloning the repository

### Option 2: Without Docker

- **Node.js**: Version 14.0 or higher ([Download Node.js](https://nodejs.org/))
- **npm**: Version 6.0 or higher (comes with Node.js)
- **Git**: For cloning the repository

## Dependencies

This project uses the following main dependencies:

### Core Dependencies

- **React** (v19.1.1): JavaScript library for building user interfaces
- **React DOM** (v19.1.1): React package for working with the DOM
- **React Scripts** (v5.0.1): Configuration and scripts for Create React App
- **Web Vitals** (v2.1.4): Library for measuring web performance metrics

### Testing Libraries

- **@testing-library/react** (v16.3.0): Testing utilities for React components
- **@testing-library/jest-dom** (v6.8.0): Custom Jest matchers for DOM elements
- **@testing-library/user-event** (v13.5.0): User interaction simulation for tests
- **@testing-library/dom** (v10.4.1): DOM testing utilities

All dependencies are automatically installed when you run `npm install` or when building the Docker container.

## Installation

### Method 1: Using Docker (Recommended)

Docker provides a consistent development environment across all platforms.

#### Step 1: Clone the Repository

```bash
git clone https://github.com/hncj811/classIntro.git
cd classIntro
```

#### Step 2: Verify Docker Installation

Check that Docker is installed and running:

```bash
docker --version
docker compose version
```

#### Step 3: Build and Run with Docker Compose

The Docker setup handles all dependency installation automatically:

```bash
docker compose up
```

**First-time setup**: Docker will:

1. Download the Node.js base image
2. Install all npm dependencies
3. Build the application container
4. Start the development server

**Subsequent runs**: Docker uses cached layers, making startup much faster.

The app will be available at **[http://localhost:3000](http://localhost:3000)**

### Method 2: Local Installation (Without Docker)

#### Step 1: Clone the Repository

```bash
git clone https://github.com/hncj811/classIntro.git
cd classIntro
```

#### Step 2: Install Dependencies

Install all required npm packages:

```bash
npm install
```

This will:

- Download and install all packages listed in [package.json](package.json)
- Create a `node_modules` directory with all dependencies
- Generate a `package-lock.json` file for version locking

#### Step 3: Verify Installation

Check that everything is installed correctly:

```bash
npm list --depth=0
```

## Running the App

### Option 1: With Docker Compose (Recommended)

#### Start the Application

```bash
docker compose up
```

This command:

- Builds the Docker image (first time only)
- Creates and starts the container
- Mounts your source code for live reloading
- Starts the development server on port 3000

Open **[http://localhost:3000](http://localhost:3000)** in your browser.

#### Run in Background (Detached Mode)

```bash
docker compose up -d
```

#### View Logs

```bash
docker compose logs -f
```

Press `Ctrl+C` to stop following logs (container keeps running).

#### Stop the Application

```bash
docker compose down
```

#### Rebuild After Changes to Docker Files

If you modify `Dockerfile` or `compose.yml`:

```bash
docker compose up --build
```

### Option 2: Without Docker

#### Start the Development Server

```bash
npm start
```

The application will:

- Open automatically in your default browser
- Run on **[http://localhost:3000](http://localhost:3000)**
- Enable hot module reloading (changes appear instantly)
- Display compile errors and warnings in the console

#### Stop the Server

Press `Ctrl+C` in the terminal.

## Available Scripts

The following npm scripts are available in the project:

### `npm start`

Starts the development server.

```bash
npm start
```

**What it does:**

- Runs the app in development mode
- Opens **[http://localhost:3000](http://localhost:3000)** in your browser
- Enables hot module reloading (changes appear without manual refresh)
- Shows lint errors and warnings in the console

**Note**: When using Docker, this command runs automatically via `docker compose up`.

### `npm test`

Runs the test suite in interactive watch mode.

```bash
npm test
```

**What it does:**

- Launches Jest test runner
- Watches for file changes and re-runs tests
- Provides interactive menu for test filtering
- Uses React Testing Library for component testing

**Common test commands:**

- Press `a` to run all tests
- Press `p` to filter by filename
- Press `t` to filter by test name
- Press `q` to quit watch mode

Learn more: [Running Tests Documentation](https://facebook.github.io/create-react-app/docs/running-tests)

### `npm run build`

Creates an optimized production build.

```bash
npm run build
```

**What it does:**

- Builds the app for production to the `build/` folder
- Bundles React in production mode
- Optimizes the build for best performance
- Minifies code and filenames include hashes for caching
- Creates static assets ready for deployment

**Output structure:**

```
build/
├── static/
│   ├── css/
│   ├── js/
│   └── media/
├── index.html
├── manifest.json
└── robots.txt
```

Learn more: [Deployment Documentation](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run eject`

⚠️ **Warning: This is a one-way operation. Once you eject, you can't go back!**

```bash
npm run eject
```

**What it does:**

- Removes the single build dependency (react-scripts)
- Copies all configuration files into your project
- Exposes webpack, Babel, ESLint configurations
- Gives you full control over the build setup

**When to use:**

- Only if you need custom webpack/Babel configuration
- When Create React App limitations become blocking
- Most projects never need to eject

**Note:** All commands except `eject` will still work after ejecting, but they'll use the copied scripts.

## Docker Configuration

The project uses Docker to provide a consistent development environment. Here's what each configuration does:

### compose.yml

```yaml
services:
  web:
    build: .                          # Build from Dockerfile in current directory
    ports:
      - "3000:3000"                   # Map port 3000 (host:container)
    volumes:
      - .:/app                        # Mount source code for live reloading
      - ./node_modules:/app/node_modules  # Persist node_modules
    env_file:
      - .env                          # Load environment variables (if exists)
    stdin_open: true                  # Keep stdin open (for interactive mode)
    tty: true                         # Allocate a pseudo-TTY
    command: npm start                # Run development server on startup
```

### Key Features

- **Port Mapping**: Container port 3000 is mapped to host port 3000
- **Volume Mounting**:
  - Source code is mounted for instant hot-reloading
  - `node_modules` is persisted to avoid reinstallation
- **Environment Variables**: Loads from `.env` file if present
- **Auto-start**: Runs `npm start` automatically when container starts

### Dockerfile

The Dockerfile (if present) defines how the container image is built, typically including:

- Node.js base image
- Working directory setup
- Package installation
- Application code copying

## Testing

Run tests to ensure code quality and functionality:

```bash
# With Docker
docker compose exec web npm test

# Without Docker
npm test
```

### Test Structure

Tests are located alongside source files with `.test.js` or `.spec.js` extensions:

- [src/App.test.js](src/App.test.js) - Example component test

### Writing Tests

Tests use Jest and React Testing Library:

```javascript
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});
```

## Building for Production

Create an optimized production build:

```bash
# With Docker
docker compose exec web npm run build

# Without Docker
npm run build
```

The production build will be created in the `build/` directory and includes:

- Minified JavaScript and CSS
- Optimized assets with cache-busting hashes
- Service worker for offline functionality (if configured)
- Static files ready to deploy to any hosting service

### Deployment Options

Deploy the `build/` folder to:

- **Vercel**: `npm i -g vercel && vercel`
- **Netlify**: Drag and drop the build folder
- **GitHub Pages**: See [deployment guide](https://create-react-app.dev/docs/deployment/#github-pages)
- **AWS S3**: Upload build folder to S3 bucket
- **Azure Static Web Apps**: Connect your repository

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

For more information about Dockerizing React applications, see [How to Dockerize a React App](https://www.docker.com/blog/how-to-dockerize-react-app/) on the Docker blog.

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
