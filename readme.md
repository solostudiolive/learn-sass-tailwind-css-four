#### Sass and Tailwind CSS Setup Guide

```
# Step 1: Create project folder and initialize npm
mkdir sass-tailwind-project
cd sass-tailwind-project
npm init -y

# Step 2: Install necessary dependencies
# Tailwind CSS, its dependencies, and Sass
npm install tailwindcss@3.4.17 postcss autoprefixer sass

# Development dependencies for build process
npm install --save-dev postcss-cli cssnano cross-env npm-run-all

# Initialize Tailwind CSS
npx tailwindcss init -p
```

#### Install concat-cli

```
# Install the concat-cli package for combining CSS files
npm install --save-dev concat-cli
```

#### tailwind.config.js

```
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./*.html"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

```
/** @type {import('tailwindcss').Config} */
module.exports = {
	content: ["./*.html", "./src/**/*.{js,ts,jsx,tsx, css, scss}"],
	theme: {
		extend: {},
	},
	plugins: [],
};
```

#### postcss.config.js

```
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
    ...(process.env.NODE_ENV === 'production' ? { cssnano: { preset: 'default' } } : {})
  }
}
```

#### package.json scripts

```
{
  "name": "sass-tailwind-project",
  "version": "1.0.0",
  "description": "A project with Sass and Tailwind CSS using separate files",
  "main": "index.js",
  "scripts": {
    "sass:build": "sass src/scss/main.scss dist/css/sass-styles.css",
    "sass:build:minify": "sass src/scss/main.scss dist/css/sass-styles.min.css --style=compressed",
    "sass:watch": "sass --watch src/scss/main.scss:dist/css/sass-styles.css",

    "tailwind:build": "npx tailwindcss -i src/tailwind/tailwind.css -o dist/css/tailwind.css",
    "tailwind:build:minify": "npx tailwindcss -i src/tailwind/tailwind.css -o dist/css/tailwind.min.css --minify",
    "tailwind:watch": "npx tailwindcss -i src/tailwind/tailwind.css -o dist/css/tailwind.css --watch",

    "css:concat": "concat-cli -f dist/css/tailwind.css dist/css/sass-styles.css -o dist/css/styles.css",
    "css:concat:minify": "concat-cli -f dist/css/tailwind.min.css dist/css/sass-styles.min.css -o dist/css/styles.min.css",

    "build": "npm-run-all sass:build tailwind:build css:concat",
    "build:minify": "npm-run-all sass:build:minify tailwind:build:minify css:concat:minify",

    "watch:sass": "npm run sass:watch",
    "watch:tailwind": "npm run tailwind:watch",
    "watch:all": "npm-run-all --parallel watch:sass watch:tailwind",

    "dev": "npm-run-all build --parallel watch:all",
    "prod": "cross-env NODE_ENV=production npm run build:minify"
  },
  "keywords": ["sass", "tailwind", "css"],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "autoprefixer": "^10.4.15",
    "postcss": "^8.4.29",
    "sass": "^1.66.1",
    "tailwindcss": "3.4.17"
  },
  "devDependencies": {
    "concat-cli": "^4.0.0",
    "cross-env": "^7.0.3",
    "cssnano": "^6.0.1",
    "npm-run-all": "^4.1.5",
    "postcss-cli": "^10.1.0"
  }
}
```

#### Create Project Structure

```
sass-tailwind-project/
├── dist/
│   └── css/
│       ├── sass-styles.css       # Compiled Sass only
│       ├── sass-styles.min.css   # Minified Sass
│       ├── tailwind.css          # Compiled Tailwind only
│       ├── tailwind.min.css      # Minified Tailwind
│       ├── styles.css            # Combined CSS
│       └── styles.min.css        # Combined minified CSS
├── src/
│   ├── scss/
│   │   ├── components/           # Reusable components
│   │   ├── utilities/            # Helper classes
│   │   ├── pages/                # Page-specific styles
│   │   └── main.scss             # Main Sass entry point
│   └── tailwind/
│       └── tailwind.css          # Tailwind directives
├── index.html
├── package.json
├── postcss.config.js
└── tailwind.config.js
```

#### src/tailwind/tailwind.css

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

#### src/scss/main.scss

```
// Variables
$primary-color: #3490dc;
$secondary-color: #ffed4a;
$dark-color: #333;

// Import partials
@import 'components/buttons';
@import 'components/containers';
@import 'utilities/spacing';
@import 'pages/home';
```

#### src/scss/components/\_containers.scss

```
.custom-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 1rem;
}

.card {
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}
```

#### src/scss/components/\_buttons.scss

```
.btn-custom {
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  transition: all 0.2s ease;
  background-color: $primary-color;
  color: white;

  &:hover {
    background-color: darken($primary-color, 10%);
  }
}

.btn-secondary {
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  transition: all 0.2s ease;
  background-color: $secondary-color;
  color: $dark-color;

  &:hover {
    background-color: darken($secondary-color, 10%);
  }
}
```

#### src/scss/utilities/\_spacing.scss

```
.spacing-sm {
  margin-bottom: 0.5rem;
}

.spacing-md {
  margin-bottom: 1rem;
}

.spacing-lg {
  margin-bottom: 2rem;
}
```

#### src/scss/pages/\_home.scss

```
.hero {
  padding: 4rem 0;
  background-color: lighten($primary-color, 40%);

  h1 {
    color: $dark-color;
    font-size: 2.5rem;
    font-weight: bold;
    margin-bottom: 1rem;
  }

  p {
    color: lighten($dark-color, 20%);
    font-size: 1.2rem;
    max-width: 600px;
    margin: 0 auto;
  }
}
```

#### index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sass & Tailwind Project (Separate Files)</title>

  <!-- You can use either the combined CSS file -->
  <link rel="stylesheet" href="dist/css/styles.css">

  <!-- Or you can load the files separately for development -->
  <!--
  <link rel="stylesheet" href="dist/css/tailwind.css">
  <link rel="stylesheet" href="dist/css/sass-styles.css">
  -->
</head>
<body class="bg-gray-100">
  <div class="hero">
    <div class="custom-container text-center">
      <h1>Sass & Tailwind CSS Project</h1>
      <p>This is a starter template using separate Sass and Tailwind CSS files.</p>
    </div>
  </div>

  <div class="custom-container py-8">
    <div class="max-w-md mx-auto bg-white card p-6">
      <p class="spacing-md">This is a sample project using:</p>
      <ul class="list-disc pl-5 spacing-md">
        <li>Tailwind CSS for utility classes</li>
        <li>Sass for custom component styles</li>
      </ul>
      <div class="flex space-x-4">
        <button class="btn-custom">Sass Button</button>
        <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
          Tailwind Button
        </button>
      </div>
    </div>
  </div>
</body>
</html>
```

#### Guide

1. **Modular organization**: Sass files are divided into components, utilities, and pages
2. **Clean separation**: Tailwind utilities are kept separate from your custom Sass styles
3. **Flexibility**: You can build and watch each technology independently
4. **Combined output option**: Final build combines both into a single file

### Available Commands:

#### Sass Commands:

-   `npm run sass:build` - Builds Sass files to dist/css/sass-styles.css
-   `npm run sass:build:minify` - Builds minified Sass files
-   `npm run sass:watch` - Watches Sass files for changes
-   `npm run watch:sass` - Alias for sass:watch

#### Tailwind Commands:

-   `npm run tailwind:build` - Builds Tailwind CSS to dist/css/tailwind.css
-   `npm run tailwind:build:minify` - Builds minified Tailwind CSS
-   `npm run tailwind:watch` - Watches Tailwind config and CSS input
-   `npm run watch:tailwind` - Alias for tailwind:watch

#### Concat Commands:

-   `npm run css:concat` - Combines Tailwind and Sass CSS files
-   `npm run css:concat:minify` - Combines minified Tailwind and Sass files

#### Combined Workflow Commands:

-   `npm run build` - Builds both Sass and Tailwind, then combines them
-   `npm run build:minify` - Builds minified versions, then combines them
-   `npm run watch:all` - Watches both Sass and Tailwind files simultaneously
-   `npm run dev` - Builds once, then watches all files (recommended for development)
-   `npm run prod` - Production build with minification
