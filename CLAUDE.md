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

## Content Writing Style Guide

### Our WHY: The Foundation of Everything We Write

**WHY we're building Grey:**
- We want to bring back the best features of old classics (Black & White, Battle for Middle-earth)
- We love gameplay over looks - substance matters more than flashy graphics
- We want to mix known features and create something new and exciting

This WHY should underpin all content decisions and messaging.

### Writing Tone & Voice

**Target Audience:** Both indie developers and potential players
**Tone:** Down-to-earth, relatable, and passionate about game development
**Voice:** Professional but approachable - avoid overly personal anecdotes

### Blog Post Structure & Style

**Opening Hook Strategy:**
- Start with concrete problems or challenges (e.g., "3 FPS with 200 units")
- Frame technical posts as development breakthroughs/solutions
- Connect technical challenges to player experience impact

**Content Balance:**
- Keep technical depth but make it accessible through analogies
- Explain the "why" behind technical decisions, not just the "how"
- Include visual storytelling with existing GIFs/videos
- Focus on what breakthroughs enable for gameplay

**Writing Guidelines:**
- Use active voice and clear, direct language
- Break up technical sections with accessible explanations
- Include concrete examples and relatable comparisons
- End with excitement about what this means for Grey's future
- Maintain professional enthusiasm without over-dramatizing

**Structure Template:**
1. **Problem/Challenge Hook** - Real development obstacle
2. **Context** - Why this matters for the game experience
3. **Solution Journey** - How we solved it (accessible technical explanation)
4. **Implementation Details** - Technical depth with clear explanations
5. **Impact & Future** - What this enables for players and Grey's development

### Content Principles

- **Transparency:** Share real development challenges and solutions
- **Accessibility:** Make technical content understandable to non-developers
- **Player Focus:** Always connect technical achievements to player experience
- **Passion:** Show genuine excitement about game development and Grey's potential
- **Authenticity:** Be honest about struggles and breakthroughs without over-sharing personal details

### Commercial Appeal Guidelines

**Market Positioning:**
- Balance nostalgia with innovation - we're not just "bringing back old games" but creating something genuinely new
- Frame problems as universal RTS frustrations, not just "modern games bad"
- Appeal to both veteran strategy fans AND newcomers to the genre
- Emphasize Grey's innovations alongside classic wisdom

**Audience Expansion:**
- Include younger gamers who might not know Black & White but understand tedious menu navigation
- Focus on universal gameplay problems (boring job assignment, static worlds) rather than generational critiques
- Position Grey as accessible to newcomers while deep enough for veterans
- Avoid language that might feel exclusionary to broader gaming audiences

**Innovation Emphasis:**
- Highlight what makes Grey unique (divine hand physics, vegetation systems, intuitive job assignment)
- Balance "classic inspiration" with "modern innovation"
- Show technical achievements as enabling better gameplay, not just solving old problems
- Position Grey as evolution, not just restoration

**Messaging Balance:**
- 40% innovation and what makes Grey special
- 30% solving universal RTS problems 
- 20% classic game inspiration
- 10% anti-modern gaming sentiment (be careful not to alienate)

**Target Expansion:**
- Primary: Strategy game enthusiasts (all ages)
- Secondary: Gamers frustrated with tedious mechanics in any genre
- Tertiary: Players interested in innovative interaction systems