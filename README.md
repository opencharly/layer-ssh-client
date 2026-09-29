# ssh-client

OpenSSH client tools for OpenCharly images — for SSH agent forwarding.

The `ssh-client` candy installs the OpenSSH client package (`openssh-clients` on
Fedora, `openssh-client` on Debian/Ubuntu, `openssh` on Arch), providing the
`ssh`, `ssh-add`, `ssh-keygen`, `scp`, and `sftp` binaries at `/usr/bin`. Boxes
compose it (directly or via the `agent-forwarding` metalayer) so in-container ssh
and git can reach hosts through the host's forwarded SSH agent socket.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `ssh-client` |
| Binaries | `/usr/bin/ssh`, `ssh-add`, `ssh-keygen`, `ssh-agent`, `scp`, `sftp` |
| Packages | RPM: `openssh-clients` · DEB: `openssh-client` · PAC: `openssh` |
| Distros | Fedora, Arch, Debian, Ubuntu |
| Service / port | none |

## How to use it

Typically composed via the `agent-forwarding` metalayer rather than directly. To
add it explicitly, pin this repo in a box's `candy:` list:

```yaml
my-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-ssh-client:v2026.239.1629'
```

With agent forwarding (`charly shell`, `charly start` direct mode), the
container's SSH commands use the host's SSH agent via a forwarded socket — no SSH
agent runs inside the container.

The candy's `plan:` asserts `ssh`, `ssh-add`, and `ssh-keygen` exist, that
`ssh -V` identifies the client as OpenSSH on stderr, and that the client package
is recorded via its per-distro `package_map`.

## Layout

- `charly.yml` — the `ssh-client:` candy entity (the
  `distro.{arch,debian,fedora,ubuntu}:` package sections and the `check:` probes)
  and the embedded `ssh-client-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-infrastructure:ssh-client`
- Metalayer: `/charly-distros:agent-forwarding`
- SSH **server**: `/charly-coder:sshd`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
