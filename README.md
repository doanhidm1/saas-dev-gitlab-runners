# SaaS DEV GitLab runner

This public repository contains GitHub-hosted runners used by the private
`snailsoft/eg-tool-saas` GitLab project.

- `saas-github-linux`: non-deploy tests and image builds.
- The runner is ephemeral and unregisters when the GitHub job ends.
- Deployment remains on the private host runner because it needs access to the
  DEV filesystem and the production SSH endpoint.
