# base-vm-dev

Duas bases Arch: **host** (máquina física) e **vm-dev** (guest de desenvolvimento). Cada uma tem o seu `archinstall/` para o JSON não sumir com o live ISO.

| Pasta | Papel |
| --- | --- |
| `vm-dev/` | Guest KVM/libvirt de desenvolvimento — **ativa agora** |
| `host/` | Host físico — ainda não |

## vm-dev/

- `archinstall/user_configuration.json` — disco, locale, pacotes, services, pós-comandos
- `archinstall/user_credentials.json` — senhas (gitignored; só no disco local)

Auth (creds local): user `fabricio` com sudo + senha root (mesmo hash).

Pós-install: `git`, `openssh`, `qemu-guest-agent`; `sshd` + `qemu-guest-agent` enabled; clone de `fabriciogferreira/git` em `/home/fabricio/git`.

### Instalar a partir do live ISO

Ordem:

1. **Guest (console do virt-manager):** `passwd` — senha do root do ISO, para o SSH do host aceitar
2. **Host:** descobrir o IP, `scp` do creds, depois `ssh`
3. **Guest (no SSH):** `archinstall …`

```bash
# Host — IP da guest no live (hostname costuma ser archiso)
virsh -c qemu:///system net-dhcp-leases default

# Host — path é do usuário fabricio neste máquina, não /root/git/...
scp ~/git/base-vm-dev/vm-dev/archinstall/user_credentials.json root@IP:/root/
ssh root@IP
```

```bash
# Guest (já no SSH)
archinstall \
  --config-url 'https://raw.githubusercontent.com/fabriciogferreira/base-vm-dev/main/vm-dev/archinstall/user_configuration.json' \
  --creds /root/user_credentials.json \
  --silent
```

Notas:

- O `scp` e o `ssh` rodam no **host**. Se rodar o `scp` dentro do ISO, `~` vira `/root` e o arquivo não existe.
- Sem o `user_credentials.json` em `/root/` na guest, o `--silent` falha na Authentication.
- Se o creds já foi copiado nesta sessão do ISO, pode pular o `scp`.
- Confirme o IP a cada boot — o lease muda.
