# Security Policy

This project calls paid third-party APIs, so security matters.

## Rules this project follows

- API keys live in environment variables only. Never commit `.env`.
- Every route except the health check requires the `x-app-token` header.
- CORS is limited to the origins you list in `ALLOWED_ORIGINS`.
- Requests are rate limited per IP.
- Reference-URL fetching refuses private, local and link-local addresses.

## Reporting a problem

Please report vulnerabilities privately through GitHub's "Report a vulnerability" option under the Security tab, or contact [Jamil Ahmed](https://jamilahmed.com). Do not open a public issue for security problems.
