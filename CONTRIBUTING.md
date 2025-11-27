# Contributing to UR20 Swarm Control Dashboard

We'd love your contributions! This document provides guidelines for contributing.

## Getting Started

1. **Fork** the repository
2. **Clone** your fork locally
3. **Create** a feature branch from `main`
4. **Make** your changes
5. **Commit** with clear messages
6. **Push** to your fork
7. **Open** a Pull Request

## Development Setup

```bash
# Clone repository
git clone https://github.com/your-username/ur20-swarm-control.git
cd ur20-swarm-control

# No additional setup needed - vanilla JavaScript project
# Just open index.html in your browser
```

## Code Style Guidelines

### JavaScript
- Use ES6+ syntax
- const/let, not var
- Arrow functions preferred
- Meaningful variable names
- Add comments for complex logic

```javascript
// ✅ Good
const getRobotStatus = (robotId) => {
    const robot = this.robots.find(r => r.id === robotId);
    return robot?.status || 'UNKNOWN';
};

// ❌ Avoid
var x = robots.find(r => r.id == robotId);
```

### CSS
- Use CSS custom properties (variables)
- Mobile-first responsive design
- Follow existing color palette
- Use semantic class names
- Group related styles

```css
/* ✅ Good */
.robot-card {
    padding: var(--space-md);
    background: var(--bg-secondary);
    border: 1px solid var(--border-color);
    border-radius: var(--radius-lg);
}

/* ❌ Avoid */
.robot-card {
    padding: 16px;
    background: #ffffff;
    border: 1px solid #ddd;
}
```

### HTML
- Use semantic elements
- Clear class names
- Data attributes for JS hooks
- Proper ARIA labels

```html
<!-- ✅ Good -->
<button 
    class="btn btn--primary" 
    data-action="start"
    aria-label="Start all robots"
>
    Start All
</button>

<!-- ❌ Avoid -->
<div onclick="start()">Start</div>
```

## Commit Message Format

```
type(scope): subject

body

footer
```

### Types
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Code style (no logic changes)
- `refactor`: Code restructuring
- `perf`: Performance improvement
- `test`: Testing additions

### Example
```
feat(console): add log filtering

Add ability to filter debug console logs by type (error, warning, info).
Implement search functionality with debouncing.

Closes #123
```

## Pull Request Process

1. **Update** CHANGELOG.md with changes
2. **Update** README.md if needed
3. **Add** descriptive PR title and description
4. **Reference** related issues
5. **Request** review from maintainers
6. **Address** feedback promptly

### PR Template
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related Issues
Closes #(issue number)

## Testing
Description of testing performed

## Screenshots (if applicable)
Add screenshots for UI changes
```

## Testing Requirements

Before submitting PR, ensure:
- [x] Code follows style guidelines
- [x] Comments added for complex logic
- [x] README updated (if needed)
- [x] No console errors
- [x] Tested on Chrome, Safari, Firefox
- [x] Responsive design verified
- [x] Dark/light theme working
- [x] All sections accessible

## Common Tasks

### Adding New Feature
1. Create feature branch: `git checkout -b feature/amazing-feature`
2. Implement in single-file structure
3. Add console logging for debugging
4. Update documentation
5. Test thoroughly
6. Submit PR

### Fixing Bug
1. Create bug branch: `git checkout -b fix/bug-description`
2. Add comment explaining fix
3. Verify fix doesn't break other sections
4. Update CHANGELOG.md
5. Submit PR

### Improving Documentation
1. Create docs branch: `git checkout -b docs/update-name`
2. Update markdown files
3. Verify formatting
4. Submit PR

## Performance Guidelines

- Keep functions under 50 lines
- Avoid nested callbacks (use arrow functions)
- Limit DOM updates per cycle
- Cache element references if used multiple times
- Use requestAnimationFrame for animations

## Accessibility Requirements

- Maintain WCAG AA compliance
- Test with keyboard navigation
- Verify color contrast (4.5:1 for text)
- Add appropriate ARIA labels
- Test with screen readers

## Questions?

- **Issues**: Use GitHub Issues for bugs/features
- **Discussions**: Use GitHub Discussions for questions
- **Email**: contact@example.com

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

---

Thank you for contributing! 🙏
