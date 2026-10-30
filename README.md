# SaaS DEV GitLab runner

This public repository contains GitHub-hosted runners used by the private
`snailsoft/eg-tool-saas` GitLab project.

- `saas-github-linux`: non-deploy tests and image builds.
- A runner accepts one GitLab job and ends with the disposable GitHub VM.
- Each workflow is hard-capped at 5h30m, below GitHub's six-hour job limit.
- GitLab job identifiers, refs, traces, and artifacts are not uploaded to
  GitHub. Runner output is redirected to a private temporary file and discarded.
- GitHub logs contain only generic runner lifecycle messages. The GitLab job
  trace remains in the private GitLab project.
- The GitLab runner binary is version-pinned and checksum-verified.
- External GitHub Actions are disabled for this repository; the workflows use
  only the preinstalled GitHub runner environment.
- Deployment remains on the private host runner because it needs access to the
  DEV filesystem and the production SSH endpoint.

The disposable VM has Docker access only for image builds. A malicious image
build can still exercise the VM's Docker daemon, so untrusted pull requests must
not be allowed to run the build tag. The VM is destroyed after the single job.
