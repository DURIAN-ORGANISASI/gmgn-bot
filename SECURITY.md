# Security policy

## Scope and limitations

GM/GN Bot handles a highly sensitive Discord user-account token in its current self-bot implementation. Treat the token like a password. This project is not an official Discord bot and its use of a user account carries platform and account risks.

## Reporting a security concern

Do not publish tokens, cookies, private logs, account identifiers, or exploit details in a public issue.

If GitHub's private vulnerability reporting is enabled for this repository, use it for sensitive reports. If it is unavailable, open a public issue asking maintainers for a private reporting channel without including sensitive details.

## Safe handling

- Store secrets only in the runtime's environment variables or approved secret store; never commit real `.env` files or tokens.
- Use the included `.env.example` only as a placeholder template.
- Limit access to Railway project variables, deployment logs, and volumes.
- Do not log the full token, message contents unnecessarily, or other account secrets.
- If a token may have leaked, invalidate it using the account provider's security controls, review active sessions, and update the deployment secret. Do not reuse an exposed token.
- Avoid sharing production logs publicly; redact credentials, private channel identifiers, and personal information first.
- Prefer an official Discord bot application and its scoped permissions for supported automation.

Discord prohibits self-bots in its [published policy guidance](https://support.discord.com/hc/en-us/articles/115002192352-Automated-User-Accounts-Self-Bots). The repository's MIT license does not change that platform policy.
