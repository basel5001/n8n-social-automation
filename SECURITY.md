# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability, please report it responsibly:

1. **Do NOT** create a public GitHub issue
2. Email: basel5001@users.noreply.github.com
3. Include details about the vulnerability and steps to reproduce

## Secrets Management

This project handles sensitive API credentials. Follow these practices:

- Never commit `.env` files (blocked by `.gitignore`)
- Use `N8N_ENCRYPTION_KEY` to encrypt credentials at rest
- Rotate API tokens regularly
- Use least-privilege access tokens for each platform

## Supported Versions

| Version | Supported |
|---------|-----------|
| Latest  | Yes       |
