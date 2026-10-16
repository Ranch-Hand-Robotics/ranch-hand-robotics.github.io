# Security reporting

For suspected security issues in this website, email **contact@ranchhandrobotics.com** rather than opening a public issue containing exploit details or sensitive data.

Include the affected URL or source file, a description of the impact, and a minimal reproduction. Do not include private keys, access tokens, payment information, or other people's personal data. Please allow an opportunity to investigate before publishing details.

This reporting channel does not authorize testing robots, third-party services, or systems you do not own or have permission to test.

## Publishing safely

- Never commit secrets or private sponsor-access content to this website repository. Build exclusions do not protect files already committed to a public repository.
- Review the built site's file list before deployment; GitHub Pages serves published artifacts publicly.
- Keep dependencies and GitHub Actions up to date. Dependabot is configured for Bundler and Actions.
- Review custom-domain HTTPS enforcement, repository protections, environment approvals, and form access settings in their respective administration consoles.