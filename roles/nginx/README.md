# Nginx lab role

For a disposable Ubuntu or Debian VM with systemd, Python 3 and SSH. Do not run against production: this installs Nginx and replaces /var/www/html/index.html. No firewall changes or TLS configuration are included.

## Test

From the repository root, install ansible-core in a virtual environment on your control node. Create an inventory.ini outside Git:

```ini
[web]
lab ansible_host=192.0.2.10 ansible_user=ubuntu
```

Replace the example address and username with your VM details. Ensure SSH and sudo work. Do not disable host-key checking.

```bash
ansible-playbook -i inventory.ini nginx-lab.yml --syntax-check
ansible-playbook -i inventory.ini nginx-lab.yml
ansible-playbook -i inventory.ini nginx-lab.yml
ansible-playbook -i inventory.ini nginx-lab.yml --check
ansible -i inventory.ini web -b -m ansible.builtin.command -a 'nginx -t'
ansible -i inventory.ini web -m ansible.builtin.uri -a 'url=http://127.0.0.1/ return_content=true'
```

Expect successful syntax validation, a working HTTP response containing Ansible Arena, and zero changes on the immediate second run (assuming no external drift or expired apt cache). Check mode is intended after initial installation; a fresh VM may lack Nginx and its directories.

## Collaboration checklist

- MeenalJy: review role implementation and run the playbook.
- annkkiitt: independently test, contribute a validation or troubleshooting improvement, and record OS version, commands and results in the PR.
- Review both contributions before creating a genuine co-authored commit. Do not attribute work not performed.

Cleanup: destroy the disposable VM after testing. No production readiness claim is made.
