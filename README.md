# Networking

Ansible role that configures various networking components, such as DNS and
systemd-networkd.

Note that this role avoids using `systemd-resolved` entirely for DNS - it
removes it and instead configures the system to use `/etc/resolv.conf`. The
motivation for this is primarily because of the way `systemd-resolved` struggles
to handle DNS sometimes with some of my highly tuned setups. For some systems, I
know exactly what my DNS server will always be, and I don't need
`systemd-resolved`.

If you're running `NetworkManager`, it's not a good idea to use this role. Its
purpose is primarily for `systemd-networkd`.

## Requirements

There are no requirements for this, it should work out of the box with Ansible.

## Role Variables

There are a few specific variables. Please see `defaults/main.yml` for variables
that can all be set on a per-host basis.

## Dependencies

None at the moment.

## Usage

First, create a `requirements.yml` in your Ansible setup if you haven't already,
here's an example:

```yaml
---
collections:
  - name: community.general
  - name: ansible.posix
  - name: community.crypto

roles:
  - name: charles-m-knox.networking
    src: https://github.com/charles-m-knox/ansible-role-networking.git
    scm: git
    version: main
```

Next, install it:

```bash
# include the -f flag to forceably re-install and get the latest (equivalent to
# upgrading)
ansible-galaxy role install -f -r requirements.yml
```

Now, in your `site.yml` (or whatever your playbook is named), use the role -
note that root access is required for some of the steps:

```yaml
- name: networking
  hosts: all
  roles:
    - charles-m-knox.networking
```

```bash
ansible-playbook site.yml --tags networking,dns --step
```

## License

MIT

## Author Information

<https://charlesmknox.com>
