# CLAUDE.md

This project is for the homepage of the game development studio of Diener Brothers GbR. Diener Brothers currently develop "grey" a vide game inspired by the RTS/God-Game genre like Black and White.

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

This is a Hugo-based website using the Hugoplate theme. Key commands:

- `npm run dev` - Start development server with hot reload
- `npm run build` - Build production site with optimization
- `npm run preview` - Preview production build locally
- `npm run format` - Format code with Prettier

## Architecture Overview

### Hugo Static Site Structure
- **Content**: Site content in `/content/english/` (multilingual ready)
- **Theme**: Uses Hugoplate theme from `/themes/hugoplate/`
- **Configuration**: Split across `/config/_default/` directory
  - `hugo.toml` - Main Hugo configuration
  - `params.toml` - Theme parameters and site settings
  - `languages.toml` - Language configuration
  - `menus.en.toml` - Navigation structure

### Theme Customization
- **Styling**: Tailwind CSS v4.0 with custom overrides in `/assets/css/custom.css`
- **Colors/Fonts**: Configured in `/data/theme.json`
- **Layout Overrides**: Custom layouts in `/layouts/` override theme defaults

### Key Features
- Dark/light theme switching (currently disabled in config)
- Search functionality powered by Hugo's built-in search
- Multilingual support (English primary, German configured)
- Image optimization with WebP generation
- Social media integration via `/data/social.json`

### Content Management
- Pages in `/content/english/pages/` for static content
- Navigation managed in `config/_default/menus.en.toml`
- SEO metadata configured in `config/_default/params.toml`

### Build Process
- Hugo generates static files to `/public/`
- Tailwind CSS compilation integrated
- Image processing with multiple format generation
- Template metrics enabled for optimization insights