# SSH-Hardening

Harden the default SSH configuration.

Keys are generated locally because you should never trust a third
party to generate your keys (broken PRNG or bad hardware).

**WARNING:** this role will disable root login from SSH.  Make sure
`su` or `sudo` is installed and configured properly.  Otherwise, who
might lock you out from configuring your system.

## Requirements

Python installed on the server.

## Role Variables

Available customizations:

 - `ssh_hardening_config_file`: path to the sshd configuration file (default `/etc/ssh/sshd_config"`)
 - `ssh_hardening_host_key_remote_path`: path to host keys (default `/etc/ssh`)
 - `ssh_hardening_host_key_temp_path`: temporary path on the local machine to generate SSH host keys.  Make sure this directory is on memory and not disk, otherwise private keys might be recovered if the disk is stolen.  (default: `/tmp`)
 - `ssh_hardening_permit_root_login`: whether to permit root login (default `no`)
 - `ssh_hardening_users_allowed`: list of users allowed to connect to the server (default none)
 - `ssh_hardening_groups_allowed`: list of groups allowed to connect to the server (default `ssh-allowed`)
 - `ssh_hardening_client_alive_interval`: idle time before automatic disconnect (default `120`)
 - `ssh_hardening_sftp_server_bin`: sftp server executable (default empty since differ between distros)
 - `ssh_hardening_sftp_log_facility`: syslog facility for sftp (default: `AUTH`)
 - `ssh_hardening_sftp_log_level`: syslog log level for sftp (default: `INFO`)
 - `ssh_hardening_kex_algorithms`: key exchange algorithms to use (bad idea to change)
   - `curve25519-sha256@libssh.org`
   - `diffie-hellman-group-exchange-sha256`
 - `ssh_hardening_ciphers`: symmetric cryptography algorithms to use (bad idea to change)
   - `chacha20-poly1305@openssh.com`
	 - `aes256-gcm@openssh.com`
	 - `aes128-gcm@openssh.com`
	 - `aes256-ctr`
	 - `aes192-ctr`
	 - `aes128-ctr`
 - `ssh_hardening_macs`:  MAC (Message Authentication Code) algorithms to use (bad idea to change)
   - `hmac-sha2-512-etm@openssh.com`
   - `hmac-sha2-256-etm@openssh.com`
   - `umac-128-etm@openssh.com`
 - `ssh_hardening_host_keys`: list of host keys to generate locally on the machine.  In the form of:
 
		type: "ed25519"
		file: "ssh_host_ed25519_key"
		comment: "Ed25519"
		passphrase: ""
		rounds: 64
		fingerprint: "hostname Ed25519 fingerprint: 256 SHA256:eaC3QlTEo3qm2KAFvtQgaAv5wgM90Y/oJMmwnpbehSo hostname Ed25519 (ED25519)"

Note that you should add `fingerprint` only if the keys have already been generated.  This field serves two things:
 - avoid overwriting existing keys
 - ~~check the signatures didn’t change~~ (_not implemented yet_)

## Example Playbook

Including an example of how to use your role (for instance, with
variables passed in as parameters) is always nice for users too:

	- hosts: servers
      roles:
         - role: notetiene.ssh-hardening

## License

MIT

## Author Information

This role was created by [Etienne Prud’homme](https://www.etienne.cc/).
