# SaaS DEV GitLab runner

This public repository contains GitHub-hosted runners used by the private
`snailsoft/eg-tool-saas` GitLab project.

- `saas-github-linux`: non-deploy tests without a Docker socket.
- `saas-github-build`: trusted branch image builds with Docker socket access.
- A runner accepts one GitLab job and ends with the disposable GitHub VM.
- Each workflow is hard-capped at 5h30m, below GitHub's six-hour job limit.
- GitLab job identifiers, refs, traces, and artifacts are not uploaded to
  GitHub. Runner output is redirected to a private temporary file and discarded.
- GitHub logs contain only generic runner lifecycle messages. The GitLab job
  trace remains in the private GitLab project.
- The GitLab runner binary is version-pinned and checksum-verified.
- External GitHub Actions are disabled for this repository; the workflows use
  only the SHA-pinned `actions/cache` restore/save actions.
- The pnpm store is mounted into the job from a GitHub Actions cache keyed by
  the GitLab commit SHA. Only downloaded package archives are cached; source,
  job traces, and artifacts are never included.
- At most 16 SaaS runner jobs are dispatched concurrently, leaving capacity for
  the dispatcher and other release repositories under the account limit.
- Deployment remains on the private host runner because it needs access to the
  DEV filesystem and the production SSH endpoint.

Only the trusted `saas-github-build` lane receives the Docker socket. A malicious
image build can still exercise that disposable VM's Docker daemon, so untrusted
pull requests must not be allowed to run the build tag.
