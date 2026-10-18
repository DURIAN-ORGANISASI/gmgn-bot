# Sources, attribution, and licensing

This page documents the upstream technical references and third-party components used by GM/GN Bot. Referenced documentation does not imply endorsement by Discord, Railway, Docker, Python, or any dependency maintainer.

## Official platform and deployment references

- [Discord Developer Documentation](https://discord.com/developers/docs): official reference for applications, bots, events, and permissions.
- [Discord Terms of Service](https://discord.com/terms): platform terms. This repository currently uses a user-account client (self-bot), not an official Discord bot account.
- [Discord Support — Automated User Accounts (Self-Bots)](https://support.discord.com/hc/en-us/articles/115002192352-Automated-User-Accounts-Self-Bots): Discord's published policy guidance relevant to the current implementation.
- [Python documentation](https://docs.python.org/3/): runtime and standard-library reference.
- [pytz upstream repository](https://github.com/stub42/pytz): timezone dependency source; the project pins `pytz==2024.1`.
- [discord.py-self upstream repository](https://github.com/dolfies/discord.py-self): client library source pinned in `pyproject.toml` and `requirements.txt`. This is a third-party fork for user accounts; it is not the official Discord bot API.
- [Railway Volumes](https://docs.railway.com/reference/volumes) and [Railway variables](https://docs.railway.com/guides/variables): deployment and persistent configuration references.
- [Docker documentation](https://docs.docker.com/): container build and runtime reference.
- [GitHub documentation — repository licensing](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository): license display and reuse guidance.

## Project artwork

The SVG files under `docs/` are documentation illustrations and interface mockups. They are not live screenshots from Discord or Railway. They are stored in this repository as project documentation assets.

## Third-party names and trademarks

Discord, Railway, Docker, Python, and related names or marks belong to their respective owners. GM/GN Bot is an independent project and is not affiliated with or endorsed by Discord Inc. or Railway.

No trademark rights are granted by the project's MIT license. If new external images, fonts, code, or other assets are added, record their source and redistribution terms here before merging them.

## License scope

The root [`LICENSE`](../LICENSE) file is authoritative for this repository's own material and retains its existing MIT license and copyright notice. The MIT license does not override platform terms, grant access to private services, or relicense dependencies. Third-party dependencies remain subject to their own license and terms; consult their upstream repositories before redistributing them.
