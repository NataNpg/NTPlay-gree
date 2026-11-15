# NTPlay-gree — resumo técnico

App de **IPTV para Android TV**, leve, derivado do `tv-native` do [NTPlay](https://github.com/natanpg/NTPlay).
Repositório próprio **NataNpg/NTPlay-gree**. Pacote **`com.natanael.ntplaygree.tv`** (instala ao lado do NTPlay, sem conflito).

## Motor de vídeo
**ExoPlayer (Media3 1.3.1) + decodificador FFmpeg** (`org.jellyfin.media3:media3-ffmpeg-decoder`).
Escolhido porque roda **4K/H.265 e 1080 cheio** no aparelho — o libVLC (tentado antes) capava em 720 e falhava no 4K neste box. `LiveLoop.kt` costura canais ao vivo entregues em pedaços.

## O que muda em relação ao NTPlay (tv-native)
- **Standalone:** sem login/conta obrigatória (`locked = false`), sem telemetria de presença.
- **applicationId próprio**, instala em paralelo ao NTPlay.
- **Tema escuro "cinema"** (preto neutro) com **cor de destaque escolhível** em Ajustes → "Cor do app": Âmbar (padrão), Turquesa, Roxo, Azul — reativo e salvo.
- **Menu lateral (NavRail)** no lugar das abas no topo.
- **Abas enxutas:** Início, Ao vivo, Filmes, Séries, Favoritos. **Sem Futebol, sem Animes.**
- **Início:** Continuar assistindo + **grupos criados** (grupos personalizados do usuário) + Favoritos. **Sem recomendações automáticas** (não busca TMDB/etc.).
- **Mais leve:** não carrega Trending nem logos de canais em segundo plano.
- **Logo próprio:** "N" âmbar com um play recortado em creme (ícone, banner da TV, logo no app, splash).
- **Player (estilo moderno):** transporte **centralizado na base** — ⏮ anterior · ⏯ · ⏭ próximo (episódio); no ao vivo, ▲/▼ de canal. O avanço/retrocesso é **só pela barra de tempo**. Legenda, áudio e **⋯ Mais** (velocidade, tela, favoritar, sair) à direita. Topo limpo (título + relógio).

## Estrutura do projeto
- Projeto Gradle na **raiz** do repo; módulo `app/`.
- Kotlin em `app/src/main/java/com/natanael/ntplay/tv/`:
  - `UI.kt` — telas, `NavRail`, Início (`Recom`), grade/categorias, diálogos, **cores do tema**.
  - `AppState.kt` — estado, abas (`TABS`), grupos personalizados, progresso, favoritos.
  - `Player.kt` — player ExoPlayer (controles, barra, EPG, painéis Guia/Canais).
  - `Store.kt` — preferências (SharedPreferences): buffer, cor do app, etc.
  - `Library.kt` / `Xtream.kt` / `Net.kt` — leitura M3U/Xtream, metadados.
  - `Epg.kt`, `Futebol.kt`, `Trending.kt`, `Logos.kt`, `Sync.kt`, `Updater.kt`, `LocalServer.kt` — módulos (alguns dormentes nesta versão).
- **Cores do tema centralizadas em `UI.kt`:** `Pink`/`Cyan`/`FocusBg` derivam do acento escolhido (`accentIdx` + lista `Accents`); trocar recolore o app todo.
- Recursos em `app/src/main/res/` (ícones mipmap, `drawable/banner.png`, `drawable-nodpi/logo.png`, `splash.jpg`).

## Compilação e publicação
- **Sem build local** — tudo pelo **GitHub Actions** (`.github/workflows/build.yml`).
- Push em `app/**` ou nos arquivos gradle dispara a compilação.
- APK publicado na release **`tv-latest`** (arquivo **`NTPlay-gree.apk`**, ~11 MB, só ABIs arm).
- **Assinatura fixa:** `signing/ntgree.jks` (chave no repo, app pessoal) → o app **atualiza por cima sem conflito**. Na 1ª troca da versão debug para esta, é preciso desinstalar uma vez.
- **TMDB** (capas/sinopses) opcional via Secret `TMDB_KEY`.

## Atualizar o app na TV
Baixar `NTPlay-gree.apk` da release `tv-latest` e instalar por cima — o auto-update do app aponta para essa release.

## Observações
- Nada é testado em emulador; o usuário testa na TV e reporta (fotos da tela de erro ajudam).
- **Tecnologias:** Kotlin · Jetpack Compose (Material3) · ExoPlayer/Media3 + FFmpeg · OkHttp · Coil · ZXing (QR). minSdk 21 · targetSdk 34.
