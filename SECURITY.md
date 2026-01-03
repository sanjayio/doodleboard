# Security Policy

## Supported Versions

Doodleboard is a single-page application served directly from the `main` branch. We only support the latest deployed version.

| Version | Supported          |
| ------- | ------------------ |
| Latest  | :white_check_mark: |

## Security Model

Doodleboard is designed with security in mind:

- **No server-side processing** - All data stays in your browser
- **No external dependencies** - Reduces supply chain attack surface
- **Plaintext-only editing** - Uses `contenteditable="plaintext-only"` to prevent HTML/script injection
- **Local storage** - Content is stored in URL hash and localStorage, never transmitted to servers
- **No cookies or tracking** - Zero data collection

## Reporting a Vulnerability

If you discover a security vulnerability, please report it responsibly:

1. **Do not** open a public GitHub issue for security vulnerabilities
2. Email the maintainer directly or use [GitHub's private vulnerability reporting](../../security/advisories/new)
3. Include:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if any)

### What to Expect

- **Acknowledgment**: Within 48 hours
- **Status update**: Within 7 days
- **Resolution**: We aim to address critical vulnerabilities within 14 days

## Scope

Security issues we're interested in:

- XSS (Cross-Site Scripting) vulnerabilities
- Data leakage from localStorage or URL handling
- Service worker security issues
- Content injection bypassing plaintext-only mode

Out of scope:

- Self-XSS (requires user to paste malicious content into their own console)
- Issues requiring physical access to the user's device
- Social engineering attacks
