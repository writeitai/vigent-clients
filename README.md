# Vigent clients

Open-source clients for Vigent, a pay-per-use legal database for external agents.

## Scope

This repository will contain the CLI, local and remote MCP integration, and
SDKs for Python and TypeScript. The hosted backend, corpus processing and
infrastructure are maintained separately. Released clients will install without
a private product checkout.

## Release status

This is the repository foundation. Client implementations, package releases and
installation commands are future work. No client package or MCP endpoint is
released by this initial setup.

## Contributing

Changes go through pull requests. Keep protocol-specific logic here and keep
hosted credentials and private source data out of Git. Follow AGENTS.md and the
installed project skills. Refresh the shared skills with:

    make install_project_skills

Shared skill source: writeitai/autoboard, canonical bootstrap bundle. Record the
source commit when refreshing and commit the skills, discovery links and lockfile
together. An Opus 5.5 review is required before merging work for this foundation.

## License

Apache-2.0; see [LICENSE](LICENSE).
