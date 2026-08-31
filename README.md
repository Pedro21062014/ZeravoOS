# ZeravoOS 24.04 (amd64)

Sistema **ZeravoOS** — uma distribuição Linux baseada em **Ubuntu 24.04 LTS**, construída como
imagem **live** (casper) e **bootável em BIOS e UEFI** (ISO híbrida).

> **ISO**: `ZeravoOS-24.04-amd64.iso` — publicada no **GitHub Release** deste repositório
> (dividida em partes, pois o GitHub limita arquivos a 2 GB por asset).
> Veja a aba **Releases** para baixar e as instruções de remontagem abaixo.

---

## Conteúdo do sistema

| Item | Configuração |
|------|--------------|
| Base | Ubuntu 24.04 LTS (noble), amd64/x86_64 |
| Desktop | Padrão do sistema (GNOME) |
| Kernel | Padrão do sistema (`linux-image-generic`, 6.8.x) |
| Tema global | Padrão do sistema |
| Wallpapers | Padrão + 2 personalizados (jpg + webp) |
| Tela de bloqueio | Padrão do sistema |
| Boot | **Plymouth com animação** (splash) |
| Tema GRUB | Padrão do sistema |
| Compressão | **zstd** |
| Idioma | Padrão do sistema (pt_BR.UTF-8) |
| Fuso | Padrão do sistema (America/Sao_Paulo) |
| Persistência | **Sim** (`persistent`) |
| Login automático | Não (usuário padrão, senha solicitada) |
| Drivers extras | **NVIDIA** (nvidia-driver + mesa-vulkan) |

### Aplicativos incluídos
- **Brave** (navegador — requer repositório de terceiros; instalado quando disponível)
- **VLC**
- **LibreOffice**
- **Terminal** (GNOME Terminal)
- **htop**
- **Central de Apps** (GNOME Software)
- **Timeshift**

### Logos
- Logo de boot e de sistema: imagem personalizada (`usr/share/iso-forge/`).

---

## Como gravar a ISO

### 1) Remontar (se baixada em partes)
A ISO foi dividida em `ZeravoOS-24.04-amd64.iso.part1/2/3`. Junte as partes:

```bash
cat ZeravoOS-24.04-amd64.iso.part* > ZeravoOS-24.04-amd64.iso
```

### 2) Gravar num pendrive (recomendado)

```bash
sudo dd if=ZeravoOS-24.04-amd64.iso of=/dev/sdX bs=4M status=progress
sync
```

> Use a opção **"Write"** do *Rufus* (Windows) ou `balenaEtcher`.

### 3) Bootar
- Entre no boot do PC, selecione o pendrive (UEFI ou BIOS).
- Escolha **"ZeravoOS (Live)"** no menu GRUB.

---

## Persistência
O boot usa o parâmetro **`persistent`** (casper). Para salvar dados na sessão, crie uma
partição/arquivo com rótulo `casper-rw` no mesmo pendrive. Semanticamente simples: use
`persistence` via partição `casper-rw` (ou `writable`) ao lado da ISO.

---

## Como foi construída
Construída com a ferramenta **iso-forge** (npm) em modo REAL: `debootstrap` da base Ubuntu,
instalação dos pacotes via apt dentro do chroot, `mksquashfs`+`zstd` e remontagem com
`grub-mkrescue`/`xorriso -iso-level 3` (ISO híbrida bootável, suportando arquivos > 4 GB).
