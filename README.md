# 🦊 Pixel -- Module

> ⚡ Módulo de **desempenho e estabilidade** para **KernelSU**, feito para **toda a linha Galaxy A** 📱 (todos os modelos e gerações), com ajustes extras para o **Helio G99 / Mali-G57**.
> 🛡️ 100% **runtime**: nada é gravado em partições do sistema.

![versão](https://img.shields.io/badge/vers%C3%A3o-v2.0-orange)
![root](https://img.shields.io/badge/root-KernelSU-blue)
![chip](https://img.shields.io/badge/chip-Helio%20G99%20%2F%20Mali--G57-green)
![runtime](https://img.shields.io/badge/runtime-only-success)

👤 **Autor:** pixeltech._
🆔 **ID do módulo:** `Pixel-Module-🦊`

---

## 📱 Aparelhos compatíveis

🌟 O módulo é para **a linha Galaxy A inteira**, sem limite de modelo. A tabela abaixo mostra apenas **exemplos**, com o chip de cada um.

| Aparelho | Código do modelo | Chip | Observação |
|---|---|---|---|
| 🌟 **Toda a linha Galaxy A** | `SM-A***` | Helio, Exynos, Dimensity ou Snapdragon | ✅ Todos os modelos e gerações são reconhecidos na instalação |
| 📲 **Galaxy A07** | SM-A075 | Helio G99 / Mali-G57 | ⭐ Alvo principal do módulo |
| 📲 **Galaxy A06** | SM-A065 | Helio G85 | Ajustes genéricos, valores adaptados à RAM |
| 📲 **Galaxy A15** | SM-A155 / SM-A156 | Helio G99 (4G) / Dimensity 6100+ (5G) | A versão 4G é a mais próxima do alvo |
| 📲 **Galaxy A16** | SM-A165 / SM-A166 | Helio G99 (4G) / Exynos 1330 (5G) | A versão 4G é a mais próxima do alvo |
| 📲 **Galaxy A24** | SM-A245 | Helio G99 | Mesmo chip do alvo |
| 📲 **Galaxy A25** | SM-A256 | Exynos 1280 | Ajustes genéricos (caminhos de GPU e GED podem não existir) |
| 📲 **Galaxy A34** | SM-A346 | Dimensity 1080 | Ajustes genéricos |
| 📲 **Galaxy A35** | SM-A356 | Exynos 1380 | Ajustes genéricos |
| 📲 **Galaxy A53** | SM-A536 | Exynos 1280 | Ajustes genéricos |
| 📲 **Galaxy A54** | SM-A546 | Exynos 1380 | Ajustes genéricos |
| 📲 **Galaxy A55** | SM-A556 | Exynos 1480 | Ajustes genéricos |

✅ Na instalação, o módulo **detecta o modelo** e reconhece **qualquer Galaxy A** (`SM-A***`), de qualquer geração.
⚠️ Se o seu modelo não for da linha A, ele instala mesmo assim, com ajustes genéricos.
🧪 O ajuste extra é para o A07 (Helio G99 / Mali-G57). Nos demais modelos os ajustes são genéricos e **não foram testados um a um**.
🎮 Partes específicas da MediaTek (GED Boost, nós da GPU Mali via `devfreq`) só funcionam onde o kernel expõe esses caminhos. Em aparelhos Exynos ou Snapdragon o módulo ignora o que não existe e registra no log.

### 📋 Requisitos
- 🔓 Bootloader desbloqueado e **root com KernelSU**
- 📦 Aplicativo **KernelSU Manager**
- 🤖 Android moderno (recomendado Android 12 ou superior)

---

## ✨ O que o módulo faz

### 🚀 Alta performance (CPU e GPU)
- 🧠 **Governor `performance`** nos clusters da CPU (A55 e A76) enquanto a tela está ligada.
- 🎮 **GPU Mali-G57** via `devfreq`, quando o kernel expõe os nós.
- 🔥 **GED Boost** da MediaTek, tentando os caminhos mais comuns (ignora o que o kernel não expõe).
- 🧊 **Thermal Guard:** pausa tudo e restaura os valores originais se o aparelho esquentar, a bateria ficar baixa ou a tela apagar.
- ⚖️ Dois níveis: `maximo` (governor performance) e `equilibrado` (piso de 50% da frequência).
- 💾 Os valores originais ficam salvos e são restaurados ao pausar, ao desligar a função e ao remover o módulo.

### ⚡ Abertura de apps e toque
- 🏁 **App Launch Boost:** boost de CPU por 0,8 s ao abrir um app, acionado por eventos (sem consultar o tempo todo).
- 👆 **Touch Fixes:** long press mais rápido (300 ms).
- 🎯 **cpuset:** apps em segundo plano ficam nos núcleos pequenos, deixando os A76 livres para o app em uso.
- 🔧 **Compilação dos apps** (`speed-profile`) uma única vez, em baixa prioridade, só carregando.
- 📅 **Compilação semanal** (`bg-dexopt-job`) com o aparelho carregando, de tela apagada e frio.
- 📌 **Lista de apps para compilação completa** (`apps_compilar.txt`).

### 🖥️ Gráficos e tela
- 🎨 **HW Accel:** HWUI com `skiavk` (ou `skiagl`), além de props de SurfaceFlinger e EGL.
- 🔄 **VSYNC tuning:** offsets do SurfaceFlinger aplicados cedo no boot, antes dele iniciar.
- 🔒 **Trava de taxa de atualização** (90 Hz por padrão), só se o painel suportar, com reaplicação automática.
- 🚫 **Não mexe nas animações do sistema.**

### 🧠 RAM e memória
- 🧹 **Limpeza automática:** quando a RAM em uso passa de 3 GB (ou 75% da RAM total, o que for menor), libera memória.
- 🎯 **Alvo de 20% de RAM disponível:** a limpeza também dispara se a RAM disponível cair abaixo disso.
- 🛡️ **Apps protegidos:** `apps_manter.txt` (WhatsApp, Telegram, Spotify e Maps por padrão). A limpeza fecha só apps de terceiros fora dessa lista.
- 🗜️ **ZRAM zstd com 8 streams:** troca o algoritmo só no boot e só com RAM livre, com proteções.
- 📈 `swappiness` só **sobe** para 100 (nunca reduz), `page-cluster` em 0, `vfs_cache_pressure` em 70.
- 💽 `read_ahead_kb` adaptado à RAM: 256 em aparelhos com menos de 4,5 GB, 512 nos demais.
- 🗂️ Compactação de memória a cada minuto, de forma leve.
- 📊 **Histórico de RAM e temperatura** a cada 10 minutos.

### 🧼 Limpeza e armazenamento
- 🗑️ Limpeza do cache dos apps no boot.
- ✂️ **TRIM diário** do armazenamento, só carregando, de tela apagada e sem calor.
- 🧽 Remoção de lixo antigo do sistema: `anr`, `tombstones` e `dropbox` com mais de 2 dias.
- 🪵 Buffer de logs reduzido e rotação automática dos logs do módulo.

### 📲 Controle de apps
- 🐢 **Restringir apps em segundo plano** (`apps_restringir.txt`) via standby bucket.
- 🤖 **Restrição automática** de apps que o Android já marcou como sem uso (opcional).
- 🚷 **Desativar apps** (`apps_desativar.txt`): bloatware e telemetria da Samsung, **reversível** (lista vazia por padrão).
- 🌙 **Parar apps ao apagar a tela** (`apps_parar_tela.txt`, opcional).

### 📶 Rede
- 📡 Scan de Wi-Fi e Bluetooth em segundo plano desligado. O valor original é restaurado ao remover o módulo.
- 🔐 DNS privado opcional (`DNS_PRIVADO=dns.google`, por exemplo).

### 🎛️ Perfis prontos
Defina `PERFIL=` no `config.conf`:

| Perfil | CPU/GPU | Launch Boost | GED | Tela | Extras |
|---|---|---|---|---|---|
| 🔋 `economia` | desligado | ❌ | ❌ | 60 Hz | Foco em bateria |
| ⚖️ `equilibrado` | piso 50% | ✅ | ❌ | 90 Hz | Uso diário |
| 🚀 `desempenho` | governor performance | ✅ | ✅ | 90 Hz | Máximo |
| 🎮 `jogo` | governor performance | ✅ | ✅ | 90 Hz | Modo jogo + Não perturbe |

Vazio = você controla cada função no `config.conf`.

### 🎮 Modo jogo
- 🕹️ Ao abrir um jogo da lista `jogos.txt`, libera RAM antes de começar.
- 🔕 **Não perturbe** ligado durante o jogo e restaurado ao sair (`DND_JOGO=1`).

### 🔋 Economia automática
- 🪫 Com a bateria abaixo de **20%** e sem carregar: tela em **60 Hz** e extras de performance pausados.
- 🔌 Ao carregar ou recuperar bateria, tudo volta ao normal.

### 🏆 Modo benchmark
- 📱 Ao abrir um app de `benchmarks.txt` (AnTuTu, Geekbench, 3DMark), ativa a performance máxima e limpa a RAM antes.
- ⏸️ Durante o teste, a limpeza automática fica desligada para não atrapalhar.
- 🌡️ Os limites sobem para **45 °C** (bateria) e **85 °C** (sensores). O Thermal Guard continua ativo acima disso.
- ⏱️ Desliga ao sair do app e, por segurança, depois de **20 minutos**.
- ⚠️ **Não falsifica pontuação.** Ele só evita perdas por throttling e por apps em segundo plano. O G99 não chega à nota de chips de outra classe.

### 🩹 Auto-reparo
- 🔁 A cada 15 minutos corrige o que o sistema desfez: swappiness, page-cluster, scan Wi-Fi/Bluetooth, trava de tela e apps reativados após uma atualização.
- 💽 Armazenamento acima de 92%: limpa cache e lixo.
- 👀 Um supervisor reinicia a rotina de segundo plano se ela parar.
- 🧯 **Proteção contra bootloop:** se o boot não concluir 3 vezes seguidas, o módulo se desativa sozinho.

---

## 🖱️ Botão de ação (KernelSU Manager)

Toque no ícone de ação do módulo para ver:

- ✅ Status com `[OK]` e `[AVISO]` em cada função
- 📊 RAM, swap, armazenamento, bateria e temperatura
- 📈 Gráfico de RAM das últimas horas
- 🔝 Apps que mais usam RAM
- 🧹 Limpeza completa (modo jogo) com o antes e depois
- 🧾 Última limpeza automática e resultado final
- 💾 Backup automático da configuração em `Download/PzXGalaxy_backup`

Pelo terminal (como root):

```sh
sh /data/adb/modules/a07_otimizador/action.sh              # status + limpeza
sh /data/adb/modules/a07_otimizador/action.sh relatorio    # só o status
sh /data/adb/modules/a07_otimizador/action.sh restaurar    # restaura o backup
```

---

## 🗂️ Configuração

Os arquivos ficam em `/data/adb/a07_otimizador/` e **sobrevivem às atualizações do módulo**. Edite com um gerenciador de arquivos com root.

| Arquivo | Para que serve |
|---|---|
| ⚙️ `config.conf` | Liga e desliga cada função (1 = ligado, 0 = desligado) |
| 🛡️ `apps_manter.txt` | Apps que a limpeza de RAM não encerra |
| 🐢 `apps_restringir.txt` | Apps restritos em segundo plano |
| 🚷 `apps_desativar.txt` | Apps desativados (reversível) |
| 🔧 `apps_compilar.txt` | Apps com compilação completa |
| 🌙 `apps_parar_tela.txt` | Apps forçados a parar ao apagar a tela |
| 🎮 `jogos.txt` | Jogos do modo jogo |
| 🏆 `benchmarks.txt` | Apps que ativam o modo benchmark |
| 🔒 `apps_protegidos.txt` | Apps que nunca entram na restrição automática |

### 🔑 Principais chaves do `config.conf`

| Chave | Padrão | Função |
|---|---|---|
| `PERFIL` | vazio | economia, equilibrado, desempenho ou jogo |
| `ALTA_PERFORMANCE` | 1 | Alta performance de CPU/GPU |
| `PERF_NIVEL` | maximo | maximo ou equilibrado |
| `LIMITE_TEMP` | 42 | Temperatura da bateria (°C) que pausa a performance |
| `PERF_TEMP_SOC` | 75 | Temperatura dos sensores (°C) que pausa a performance |
| `LIMPEZA_AUTO` | 1 | Limpeza automática de RAM |
| `LIMITE_RAM_MB` | 3072 | RAM em uso que dispara a limpeza |
| `RAM_ALVO_PCT` | 20 | RAM disponível mínima (%) |
| `REFRESH_HZ` | 90 | Trava de taxa de atualização (0 = não mexe) |
| `HWUI_RENDERER` | skiavk | skiavk ou skiagl (use skiagl se notar falhas gráficas) |
| `ZRAM_ZSTD` | 1 | Troca do zram para zstd no boot |
| `LAUNCH_BOOST` | 1 | Boost ao abrir apps |
| `MODO_JOGO` | 0 | Limpeza ao abrir jogos da lista |
| `DND_JOGO` | 1 | Não perturbe durante o jogo |
| `ECONOMIA_AUTO` | 1 | Economia com bateria baixa |
| `BENCH_MODE` | 1 | Modo benchmark |
| `AUTO_REPARO` | 1 | Auto-reparo e proteção de bootloop |
| `NOTIFICAR` | 0 | Notificações (desligadas por padrão) |

📝 Os logs ficam em `/data/local/tmp/a07_otimizador.log` e `/data/local/tmp/a07_ram_historico.log`.

---

## 📥 Instalação

1. ⬇️ Baixe o ZIP em **Releases**.
2. 📱 Abra o **KernelSU Manager** → **Módulos** → **Instalar** e escolha o ZIP.
3. 🔌 Para a compilação inicial dos apps, deixe o celular **no carregador**.
4. 🔁 **Reinicie** o aparelho.
5. ▶️ Abra o módulo e toque na ação para conferir o status.

🔄 **Atualizar:** instale o novo ZIP por cima e reinicie. Suas configurações são mantidas.

## 🗑️ Remoção

Remova o módulo no KernelSU Manager e reinicie. Ele restaura:
- ✅ Scan de Wi-Fi/Bluetooth, DNS privado, taxa de atualização e long press
- ✅ Governors e frequências de CPU/GPU, e o cpuset
- ✅ Não perturbe (se ficou ativo por causa do modo jogo)
- ✅ Apps que o módulo desativou

---

## 🧯 Se algo der errado

| Problema | Solução |
|---|---|
| 🌀 Bootloop | Segure **Volume −** na animação de boot (modo de segurança do KernelSU) ou apague `/data/adb/modules/a07_otimizador` |
| 🎨 Falhas gráficas em apps | `HWUI_RENDERER=skiagl` no `config.conf` |
| 🔥 Aparelho esquentando | `PERF_NIVEL=equilibrado` ou `PERFIL=economia` |
| 🪫 Bateria durando pouco | `PERFIL=equilibrado` e desligue `REFRESH_HZ` (0) |
| 🧠 Problema com zram | `ZRAM_ZSTD=0` |
| 🔔 Notificações atrasadas | Tire o app de `apps_restringir.txt` e `apps_parar_tela.txt` |

---

## 🚫 O que o módulo **não** faz (de propósito)

- ❌ Não desativa o controle térmico
- ❌ Não faz overclock nem mexe em tensão
- ❌ Não desativa o SELinux nem serviços essenciais do Google/Samsung
- ❌ Não grava em `/system`, `/vendor`, `boot` ou outras partições
- ❌ Não falsifica pontuação de benchmark
- ❌ Não mexe nas animações do sistema

---

## ⚠️ Avisos importantes

- 🔬 Os ajustes dependem do kernel de cada aparelho. Alguns caminhos (GPU, GED, cpuset) podem não existir ou ser bloqueados pela Samsung. Nesse caso o módulo ignora e registra no log.
- 🔥 O modo de alta performance gasta mais bateria e esquenta mais com a tela ligada. O Thermal Guard reduz o risco, mas não o elimina.
- 🏆 O módulo ajuda o aparelho a render o máximo que o hardware permite, mas **não transforma um Helio G99 em um chip topo de linha**.
- 📉 Use por sua conta e risco. Faça backup e guarde o `boot.img` original.

---

## 🧾 Histórico de versões

### v2.0
- 🚀 Alta performance de CPU/GPU com Thermal Guard
- 🎨 HW Accel, VSYNC, GED Boost, Touch Fixes e App Launch Boost
- 🔒 Trava de 90 Hz com reaplicação automática
- 🗜️ ZRAM zstd com 8 streams e alvo de 20% de RAM
- 🎛️ Perfis prontos: economia, equilibrado, desempenho e jogo
- 🛡️ Lista de apps protegidos da limpeza e limpeza por app
- 🎮 Modo jogo com Não perturbe
- 🔋 Economia automática com bateria baixa
- 🏆 Modo benchmark
- 🩹 Auto-reparo, supervisor e proteção contra bootloop
- ✂️ TRIM diário, limpeza de lixo e compilação de apps
- 📊 Ação com status `[OK]`/`[AVISO]`, gráfico de RAM e backup da configuração
- 🔕 Sem notificações por padrão

---

## 🤝 Contribuindo

Testou em outro modelo? 🧪 Abra uma **issue** com:
- 📱 Modelo e chip
- 🤖 Versão do Android e do KernelSU
- 🧾 O conteúdo da ação (status) e do log `/data/local/tmp/a07_otimizador.log`

---

<p align="center">Feito por <b>pixeltech._</b> 🦊</p>
