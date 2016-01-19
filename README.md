# Ansible role for borgbackup

Ansible role for installing and configuring [Borg Backup](https://borgbackup.github.io/)

## Requirements

The borg repository must currently be created manually. Execute `borg init ssh://<server>:/<repopath>`, e.g.

    borg init ssh://johndoe@backup.example.org:/mnt/backups/borg

A borg repository can contain backups from multiple hosts, but only encrypted with a single key (as far as I understand
it…). Configuring encryption is currently not supported by this role, so only use trusted hosts for your backups.


## Role Variables

    borgbackup_version: '0.29.0'

The borg version to install. Currently, only released versions are supported

    borgbackup_platform: 'linux64'

The platform to install for (e.g. linux32, linux64)

    borgbackup_download_url: 'https://github.com/borgbackup/borg/releases/download/{{ borgbackup_version }}/borg-{{ borgbackup_platform }}'

The URL to download the Borg binary from

    borgbackup_hash_sha256: '...'

SHA 256 hash of the borg binary

    borgbackup_ssh_key_path: '/root/.ssh/id_rsa'
    
Path to the SSH private key to use for logging into the backup server

    borgbackup_create_default_options: '--compression zlib,6'

The default options for creating a new backup, e.g. for enabling debug output. See [the Borg documentation](https://borgbackup.readthedocs.org/en/stable/usage.html#borg-create) for more information.

    borg_backups:
      - backup_server: '' # must be defined by you
        backup_repository: '' # must be defined by you
        backup_prefix: '{{ ansible_hostname }}'
        backup_name: '`date +%Y-%m-%d`'
        directories:
          - '' # define paths like /etc here
        borg_options: '{{ borgbackup_create_default_options }}'

A list of backups to create. Each of these has the following properties:

* `backup_server`: The username and server; including the protocol, e.g. `ssh://johndoe@backup.example.org`
* `backup_repository`: The path to the repository
* `backup_prefix`: The prefix for the backup name. Defaults to `ansible_hostname`
* `backup_name`: The name of the individual backup. Should include at least the current date; it might also e.g. be the day of week
* `directories`: A list of directories to backup
* `borg_options`: The options for the backup. Defaults to `borgbackup_create_default_options`


## Dependencies

No dependencies as of now.


## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - role: andreaswolf.borgbackup


## See also

 * [Borg backup Github repository](https://github.com/borgbackup)
 * [Borg backup documentation](http://borgbackup.readthedocs.org/)


## TODO

* Encryption should be supported.
* A repository should be created if it does not exist.


## License

MIT

## Author Information

This role was created by Andreas Wolf. Visit my [website](http://a-w.io) and 
[Github profile](https://github.com/andreaswolf/) or follow me on [Twitter](https://twitter.com/andreaswo).

