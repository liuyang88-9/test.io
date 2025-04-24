# Getting Started

This guide will help you get started with using this documentation.

## Prerequisites

- Node.js version 16 or higher
- npm or yarn package manager

## Installation

1. Clone the repository:

```bash
git clone https://github.com/yourusername/your-repo.git
cd your-repo
```

2. Install the dependencies:

```bash
npm install
# or
yarn install
```

3. Start the development server:

```bash
npm run docs:dev
# or
yarn docs:dev
```

4. Open your browser and navigate to `http://localhost:5173/` to see the documentation site.

## Building for Production

To build the site for production, run:

```bash
npm run docs:build
# or
yarn docs:build
```

This will generate static HTML files in the `.vitepress/dist` directory that you can deploy to any static site hosting service.

## Customizing the Site

To customize the site, you can edit the configuration file at `.vitepress/config.js`. See the [VitePress documentation](https://vitepress.dev/reference/site-config) for more details on available options.

## Adding Content

To add new content, simply create Markdown files in the `docs` directory. The file structure will be reflected in the URL structure of your site. 