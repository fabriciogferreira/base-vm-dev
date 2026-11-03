# base-vm-dev

Duas bases Arch: **host** (máquina física) e **vm-dev** (guest de desenvolvimento). Cada uma tem o seu `archinstall/` para o JSON não sumir com o live ISO.

| Pasta | Papel |
| --- | --- |
| `vm-dev/` | Guest KVM/libvirt de desenvolvimento — **ativa agora** |
| `host/` | Host físico — ainda não |

## vm-dev/

- `archinstall/user_configuration.json` — disco, locale, pacotes, services, pós-comandos
- `archinstall/user_credentials.json` — senha root (gitignored; só no disco local)

Auth (no `user_credentials.json` local): user `fabricio` com sudo + senha root (mesmo hash).

Pós-install: `git`, `openssh`, `qemu-guest-agent`; `sshd` + `qemu-guest-agent` enabled; clone de `fabriciogferreira/git` em `/home/fabricio/fabricio`.

```bash
archinstall \
  --config-url 'https://raw.githubusercontent.com/fabriciogferreira/base-vm-dev/main/vm-dev/archinstall/user_configuration.json' \
  --creds /root/user_credentials.json \
  --silent
```

O `user_credentials.json` precisa estar em `/root/` na guest (cópia do arquivo local deste repo). Sem ele o `--silent` para na Authentication.
