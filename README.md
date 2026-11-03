# base-vm-dev

Config da VM base de desenvolvimento (Arch via archinstall). O JSON no live ISO some no reboot; aqui no host ele fica.

## archinstall/

- `user_configuration.json` — layout, bootloader, pacotes, services e pós-comandos (ok no git)
- `user_credentials.json` — senha root em hash (gitignored; não commitar)

Pós-install (via `packages` / `services` / `custom_commands`):

- `git`, `openssh`, `qemu-guest-agent`
- `sshd` e `qemu-guest-agent` habilitados
- clone de `fabriciogferreira/git` em `/root/fabricio` (home do root no install)

Sem o `creds`, o archinstall abre o menu pedindo Authentication. Para instalar sem surpresa:

```bash
archinstall \
  --config-url 'https://raw.githubusercontent.com/fabriciogferreira/base-vm-dev/main/archinstall/user_configuration.json' \
  --creds /root/user_credentials.json \
  --silent
```

O `user_credentials.json` precisa existir na guest (`/root/`). No host ele fica em `archinstall/user_credentials.json` (fora do git). Se sumir no live, copie de novo do host antes do comando acima.
