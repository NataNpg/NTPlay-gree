# NTPlay-gree

Player de **IPTV para Android TV**, leve e simples, com motor de vídeo **libVLC** e
**seletor de cor do app**. Deriva do NTPlay (app nativo de Android TV), trocando o
player ExoPlayer por libVLC e rodando **sem login obrigatório** — você põe sua lista e usa.

- **Pacote:** `com.natanael.ntplaygree.tv` (instala ao lado do NTPlay normal, sem conflito)
- **Baixar o APK (sempre a última versão):**
  https://github.com/NataNpg/NTPlay-gree/releases/download/tv-latest/NTPlay-gree.apk

## Visual

Tema escuro "cinema" (preto neutro) com **cor de destaque escolhida pelo usuário** em
Ajustes → "Cor do app": **Âmbar** (padrão), **Turquesa**, **Roxo** ou **Azul**. A escolha
fica salva e recolore tudo — foco, barra de progresso, "ao vivo", logo e botões do player.

## O que faz

- **Listas M3U e Xtream Codes** (servidor/usuário/senha). Várias listas ao mesmo tempo.
- **Passar a lista do celular por QR code**: na TV, abra **☰ Listas**; o celular (mesmo Wi-Fi)
  lê o QR e envia o link/M3U direto.
- **Navegação Android TV**: TV ao vivo, Filmes, Séries, Animes, Favoritos, Recomendações —
  foco por controle, capas oficiais (TMDB/TVmaze/iTunes), busca.
- **Player libVLC**: roda TS/HLS, canais ao vivo em pedaços, AC3/EAC3/DTS e HEVC/4K sem os
  problemas de codec do ExoPlayer. Controles próprios, guia (EPG), ajuste de tela.
- **VLC externo**: botão "Abrir em outro player (VLC/MX)" para o que o aparelho não rodar.
- **Atualização automática do próprio app** pela release `tv-latest`.

## Compilar

Tudo pelo **GitHub Actions** (`.github/workflows/build.yml`), sem ambiente local:

- Push em `app/**` ou no gradle dispara a compilação.
- O APK vai para a release **`tv-latest`** (arquivo `NTPlay-gree.apk`).
- **Assinatura:** sem segredos, usa a **chave debug** (compila e instala na hora). Para
  atualizações por cima no futuro, crie uma chave fixa e os Secrets `NT_GREE_KEYSTORE_B64`
  + `NT_GREE_KS_PASS`.
- **TMDB (opcional):** Secret `TMDB_KEY` para capas e sinopses oficiais.

## Tecnologias

Kotlin · Jetpack Compose (Material3) · **libVLC 3.6** · OkHttp · Coil · ZXing (QR) ·
minSdk 21 · targetSdk 34 · só arquiteturas arm (APK menor).

---

Autor: **Natanael G Silva**. Projeto irmão do [NTPlay](https://github.com/NataNpg/NTPlay).
