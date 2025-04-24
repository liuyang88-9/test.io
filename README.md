# My Documentation Site

This is a documentation site built with [VitePress](https://vitepress.dev/), a Vue-powered static site generator.

## Development

### Prerequisites

- Node.js version 16 or higher
- npm or yarn package manager

### Setup

1. Clone this repository
2. Install dependencies:
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
4. Visit `http://localhost:5173` to see the site

### Building

To build the site for production:

```bash
npm run docs:build
# or
yarn docs:build
```

To preview the built site locally:

```bash
npm run docs:preview
# or
yarn docs:preview
```

## Deployment

This site is automatically deployed to GitHub Pages when changes are pushed to the main branch.

## Customization

To customize the site:

1. Edit `.vitepress/config.js` to change site configuration
2. Add or modify Markdown files in the `docs` directory to add content
3. Customize styles by editing `.vitepress/theme` files (create if doesn't exist)

## License

MIT 