# Contributing to Doodleboard

Thank you for your interest in contributing to Doodleboard! We welcome contributions from the community to help make this tool better for everyone.

## Code of Conduct

By participating in this project, you agree to abide by our Code of Conduct. Please be respectful and inclusive in all interactions.

## How Can I Contribute?

### Reporting Bugs

- Check the [Issues](../../issues) to see if the bug has already been reported.
- If not, open a new issue with a clear title and description.
- Include steps to reproduce the bug, expected behavior, and screenshots if possible.

### Suggesting Enhancements

- Open an issue with the tag `enhancement`.
- Clearly describe the new feature or improvement you have in mind.
- Explain why this enhancement would be useful to users.

### Pull Requests

1. Fork the repository and create your branch from `main`.
2. Make your changes. Ensure your code follows the existing style and conventions.
3. Test your changes locally by serving the files:
   ```bash
   python3 -m http.server 8000
   # or
   npx serve
   ```
4. Verify PWA functionality and offline support work correctly.
5. If you modified the application, update the service worker cache version in `sw.js`.
6. Commit your changes with clear and descriptive commit messages.
7. Push to your fork and submit a Pull Request to the `main` branch.
8. Describe your changes in the PR description, linking to any relevant issues.

## Coding Standards

- **JavaScript**: Pure vanilla ES6+ JavaScript with no external dependencies.
- **No Semicolons**: This project uses ASI (Automatic Semicolon Insertion).
- **Self-Contained**: All functionality must remain in inline scripts within HTML files.
- **No Build Process**: The project must work without any build tools or compilation.
- **Browser APIs**: Prefer native browser APIs over external libraries.
- **PWA Compatible**: Changes must maintain offline support and service worker functionality.
- **Mobile Responsive**: Ensure proper viewport handling and touch support.
- **Dark Mode**: Support `prefers-color-scheme` for dark mode.

## Project Structure

- `index.html` - Main editor interface (self-contained with inline CSS and JS)
- `qr.html` - QR code generator for sharing URLs
- `sw.js` - Service worker for offline support
- `manifest.json` - PWA manifest

## License

By contributing to Doodleboard, you agree that your contributions will be licensed under the same license as the project.
