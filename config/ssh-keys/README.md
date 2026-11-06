# SSH login keys

Put one public key per test user here as `<user>.pub` (e.g. `user-a.pub`).
`./do_startup.sh` (via `tools/provision-qfw-cluster.sh`) installs each into
`<home>/.ssh/authorized_keys` on `slurmctld`, whose sshd is published on host
port `${QFW_SSH_PORT:-2222}`:

    ssh -p 2222 user-a@<host>

From a workstation, an `~/.ssh/config` entry makes it one hop (no login to the
host first). The port is published on the host, so no `ProxyJump` is needed
when the host is reachable directly:

    Host qfw-user-a
        HostName grumium2        # or the host's address
        Port 2222
        User user-a
        IdentityFile ~/.ssh/<private key matching config/ssh-keys/user-a.pub>
        IdentitiesOnly yes
