# base-vm-dev

Config da VM base de desenvolvimento (Arch via archinstall). O JSON no live ISO some no reboot; aqui no host ele fica.

## archinstall/

- `user_configuration.json` — layout, bootloader, pacotes (ok no git)
- `user_credentials.json` — senhas (gitignored; não commitar)

Na guest, o archinstall **não enxerga** este path do host. Salve em `/root/` no ISO e copie para cá, ou use URL depois do push:

```bash
archinstall \
  --config-url 'https://raw.githubusercontent.com/fabriciogferreira/base-vm-dev/main/archinstall/user_configuration.json' \
  --creds /root/user_credentials.json \
  --silent
```
