# Personalized Template Hugo Reveal Slides

A Hugo-based template for creating beautiful [Reveal.js](https://revealjs.com/) presentations with custom styling and extended features.

## Features

- 🎨 Custom SCSS theme with personalized colors and fonts
- 📊 Mermaid diagram support with inline rendering
- 📱 Multi-column layout support via shortcodes
- 🔧 Custom shortcodes for enhanced slide creation
- 🚀 Automated build and deployment via GitHub Actions
- 📐 High resolution slides (1920x1080)

## Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (v0.152.2 or later recommended)
- [Ruby](https://www.ruby-lang.org/) (for the slide preprocessor)

## Getting Started

1. **Clone the repository with submodules:**

   ```bash
   git clone --recurse-submodules https://github.com/angelacorte/Personalized-Template-Hugo-Reveal-Slides.git
   cd Personalized-Template-Hugo-Reveal-Slides
   ```

2. **Run the preprocessor:**

   ```bash
   ./shared-slides/preprocess.rb
   ```

3. **Start the Hugo development server:**

   ```bash
   hugo server
   ```

4. **Open your browser** at `http://localhost:1313` to view the presentation.

## Project Structure

```
├── assets/
│   └── custom-theme.scss    # Custom SCSS theme
├── content/
│   └── _index.md            # Main presentation content
├── layouts/
│   ├── partials/            # Hugo partials
│   └── shortcodes/          # Custom shortcodes
├── shared-slides/           # Shared slides submodule
├── static/                  # Static assets (images, CSS)
├── themes/
│   └── reveal-hugo/         # Reveal-Hugo theme submodule
└── config.toml              # Hugo configuration
```

## Customization

### Theme Configuration

Edit `config.toml` to customize Reveal.js settings:

```toml
[params.reveal_hugo]
theme = "league"
highlight_theme = "solarized-dark"
transition = "slide"
transition_speed = "fast"
```

### Custom Styling

Modify `assets/custom-theme.scss` to change:

- Colors (`$backgroundColor`, `$mainColor`, `$headingColor`)
- Fonts (`$mainFont`, `$headingFont`)
- Code styling
- Blockquote appearance
- And more

### Available Shortcodes

- `multicol` / `col` - Create multi-column layouts
- `rectdiv` - Styled content containers
- `chart` - Chart integration
- `qrcode` - QR code generation
- `import` - Import content from other files
- And many more in `layouts/shortcodes/`

## Building for Production

Build the static site:

```bash
hugo
```

The output will be in the `build/` directory.

## Deployment

This template includes a GitHub Actions workflow (`.github/workflows/build-and-deploy.yml`) that automatically:

1. Builds the presentation on push to the main branch
2. Transforms and inlines Mermaid charts
3. Deploys to GitHub Pages

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## Author

**Angela Cortecchia** - [angela.cortecchia@unibo.it](mailto:angela.cortecchia@unibo.it)
