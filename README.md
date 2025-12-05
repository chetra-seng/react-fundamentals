# React Course - Slidev Presentation

This presentation uses Slidev - a modern presentation tool for developers.

## Running the Presentation

```bash
# Install dependencies (if not already installed)
pnpm install

# Start the development server
pnpm run dev
```

The presentation will open in your browser at `http://localhost:3030`

## Building for Production

```bash
# Build static files
pnpm run build

# Export to PDF
pnpm run export
```

## Navigation & Controls

### Basic Navigation
- **Arrow keys** or **Space**: Next/Previous slide
- **Home/End**: First/Last slide
- **Number + Enter**: Jump to specific slide

### Special Features
- **F**: Toggle fullscreen
- **O**: Toggle overview mode (see all slides)
- **D**: Toggle dark mode
- **G**: Toggle drawing mode (presenter)
- **C**: Toggle camera view
- **P**: Open presenter notes in new window

## Code Highlighting Features

Slidev has amazing code highlighting with click-through animations:

### Line Highlighting
Used for focusing on specific parts of static code:

```markdown
\`\`\`jsx {2,5|7-9|all}
// Click to highlight line 2 and 5
// Then click to highlight lines 7-9
// Then click to highlight all
\`\`\`
```

### Magic Move (Slide-in Animations)
Animate code changes between slides - code morphs and new lines slide in smoothly:

````markdown
\`\`\`\`md magic-move
\`\`\`jsx
function Counter() {
  const [count, setCount] = useState(0);
}
\`\`\`

\`\`\`jsx
function Counter() {
  const [count, setCount] = useState(0);

  return <div>Count: {count}</div>;
}
\`\`\`
\`\`\`\`
````

**Where Magic Move is used in this presentation:**
- Problem vs Solution: Direct DOM Manipulation
- State Out of Sync vs Single Source of Truth
- Using useState hook example

## Project Structure

```
react-course/
├── slides.md              # Main presentation file
├── package.json           # Dependencies and scripts
└── README.md              # This file
```

## Customization

### Theme
The presentation uses the default theme. You can change it in the frontmatter of `slides.md`:

```yaml
---
theme: default
# Other available themes: seriph, apple-basic, etc.
---
```

### Background
Change the background image in the frontmatter:

```yaml
---
background: https://your-image-url.com/image.jpg
---
```

## Features Used in This Presentation

- ✅ Code syntax highlighting with Shiki
- ✅ Line-by-line code highlighting animations
- ✅ Multiple layouts (center, default, end)
- ✅ MDC (Markdown Component) syntax
- ✅ Transitions between slides
- ✅ Dark mode support
- ✅ Presenter mode

## Tips

1. **Presenter Mode**: Press `P` to open presenter notes in a new window
2. **Drawing Mode**: Press `G` to draw on slides during presentation
3. **Export PDF**: Run `pnpm run export` to create a PDF version
4. **Code Focus**: Use `{lines}` syntax to highlight specific code lines with animations

## Learn More

- [Slidev Documentation](https://sli.dev/)
- [Slidev GitHub](https://github.com/slidevjs/slidev)
- [Code Highlighting Guide](https://sli.dev/features/line-highlighting)
