# Java
Desenvolvimento na Plataforma Android,  Desenvolver aplicativos usando a plataforma Android

# 🕹️ Levantamento: build do AOSP 14 e controle arcade

![Android](https://img.shields.io/badge/AOSP_14-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry_Pi_4-A22846?style=for-the-badge&logo=raspberrypi&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu_22.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnubash&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![USB](https://img.shields.io/badge/Controle_USB_HID-005BBB?style=for-the-badge&logo=usb&logoColor=white)
![RetroArch](https://img.shields.io/badge/Emulador-RetroArch-000000?style=for-the-badge&logo=retroarch&logoColor=white)

**👤 Responsável:** [@seu-usuario]
**🛤️ Trilha:** [Embarcado / AOSP]
**📌 Status:** ![status](https://img.shields.io/badge/status-em_levantamento-yellow?style=flat-square)

---

## 🎯 1. Metas e objetivos

- 🧱 Ter um ambiente reproduzível de build do AOSP 14 (`android-14.0.0_r67`) com os manifests do Raspberry Vanilla.
- 🔌 Integrar o repositório `Hands-On-Fliperama` em `device/Hands-On-Fliperama` e aplicar os patches do projeto.
- 💿 Gerar a imagem do alvo `aosp_rpi4_fliperama-ap2a-[variant]` (boot, system e vendor) e confirmar que ela funciona na Raspberry Pi 4.
- 🎮 Fazer o controle arcade (manete e botões) se comunicar com o emulador rodando sobre o AOSP.

---

## 🛠️ 2. Atividades e subatividades

### ![Ubuntu](https://img.shields.io/badge/-A._Ambiente_de_build-E95420?style=flat-square&logo=ubuntu&logoColor=white)
- [ ] Preparar a máquina (Ubuntu 22.04, RAM, swap e disco suficientes)
- [ ] Instalar dependências e a ferramenta `repo`
- [ ] Executar `repo init` na tag `android-14.0.0_r67`
- [ ] Adicionar o manifest local do Raspberry Vanilla
- [ ] Executar `repo sync`

### ![Git](https://img.shields.io/badge/-B._Integração_do_projeto-F05032?style=flat-square&logo=git&logoColor=white)
- [ ] Clonar o repositório em `device/Hands-On-Fliperama`
- [ ] Executar `./scripts/apply_patches.sh`
- [ ] Verificar o estado com `repo status`

### ![Android](https://img.shields.io/badge/-C._Compilação-3DDC84?style=flat-square&logo=android&logoColor=white)
- [ ] `source build/envsetup.sh` e `lunch aosp_rpi4_fliperama-ap2a-[variant]`
- [ ] `make bootimage systemimage vendorimage`
- [ ] Gerar a imagem gravável e gravar no cartão SD
- [ ] Dar boot na Pi 4 e registrar as evidências (log do build, boot, `adb devices`)

### ![Raspberry Pi](https://img.shields.io/badge/-D._Controle_arcade-A22846?style=flat-square&logo=raspberrypi&logoColor=white)
- [ ] Decidir com a trilha de Embarcado entre encoder USB e GPIO direto
- [ ] Validar a leitura dos eventos com `adb shell getevent -lt`
- [ ] Criar o mapeamento de teclas (`.kl`) se necessário
- [ ] Testar o controle dentro do emulador
- [ ] (Se for GPIO) desenvolver o serviço que injeta eventos via `uinput`, com as permissões de SELinux

### ![GitHub](https://img.shields.io/badge/-E._Documentação-181717?style=flat-square&logo=github&logoColor=white)
- [ ] Registrar comandos, versões usadas, erros encontrados e soluções

---

## ⚠️ 3. Problemas, barreiras e obstáculos previstos

| Etapa | Problema provável | Mitigação |
|---|---|---|
| 🐧 Ambiente | Pouco disco ou RAM (build muito pesado) | SSD com 300 GB ou mais, swap de 16 GB ou mais, `--depth=1` no `repo init` |
| 🐧 Ambiente | Queda de rede no `repo sync` | Repetir o comando (ele retoma), reduzir o `-j` |
| 📦 Manifests | Manifest do Raspberry Vanilla incompatível com a tag `r67` | Conferir a branch `android-14.0` e fixar a revisão no manifest do projeto |
| 🩹 Patches | `apply_patches.sh` falha ou não é idempotente | Garantir árvore limpa antes (`repo status`) e documentar o procedimento de reset |
| 🤖 Build | Processo morto por falta de memória | Reduzir o `-j`, aumentar o swap |
| 🤖 Build | `lunch` não reconhece o alvo | Reabrir o terminal, refazer o `envsetup.sh`, conferir o caminho em `device/` |
| 🤖 Build | Erros de Soong ou neverallow de SELinux | Ler o primeiro erro do log, revisar os patches aplicados |
| 🤖 Build | Tempo de compilação alto | Habilitar `ccache` |
| 🍓 Boot | Pi 4 não inicializa ou não dá vídeo | Verificar fonte (5V/3A), cabo HDMI, regravar o cartão, usar console serial (UART) |
| 🎮 Controle | Android não reconhece botões ou eixos corretamente | Usar encoder USB (HID nativo), ajustar o `.kl` |
| 🎮 Controle | GPIO não vira tecla no Android | Serviço com `libgpiod` e `uinput`, ou preferir encoder USB |
| 🎮 Controle | Emulador não responde ao controle | Configurar o mapeamento de entrada no próprio emulador |
| 👥 Equipe | Decisões de hardware que afetam o software | Alinhar cedo com a trilha de Embarcado (tipo de controle, pinagem, tela) |

---

## 📢 4. Divulgação

Este levantamento será compartilhado com toda a equipe do projeto final e atualizado conforme surgirem novos obstáculos.

## ✅ 5. Critérios de aceitação

- [ ] Metas e objetivos definidos
- [ ] Atividades e subatividades definidas
- [ ] Problemas e barreiras levantados
- [ ] Levantamento divulgado para toda a equipe
