# NTPlay-gree

App de IPTV para Android TV (Kotlin + Jetpack Compose). Pacote `com.natanael.ntplaygree.tv`.
Resumo técnico completo em `docs/NTPlay-gree.md` (é a fonte mais atual; o README ainda cita libVLC).

## Stack
- Kotlin 1.9.24, Jetpack Compose (Material3), Gradle (Kotlin DSL), AGP 8.5.2, Java 17.
- minSdk 21, targetSdk 34, só ABIs arm (`armeabi-v7a`, `arm64-v8a`).
- Vídeo: ExoPlayer (Media3 1.3.1) + decodificador FFmpeg (`org.jellyfin.media3`). O libVLC foi
  testado e descartado (limitava em 720p e falhava no 4K). Não reintroduzir sem pedido.
- Rede/imagens: OkHttp, Coil. QR code: ZXing. Segundo plano: WorkManager.
- Não há banco de dados nem backend: preferências em SharedPreferences (`Store.kt`).

## Estrutura
- Projeto Gradle na raiz, módulo `app/`.
- Código em `app/src/main/java/com/natanael/ntplay/tv/` (o namespace é `ntplay.tv`, diferente do
  applicationId; não renomear).
  - `UI.kt` telas e cores do tema · `AppState.kt` estado e abas · `Player.kt` player
  - `Store.kt` preferências · `Library.kt`/`Xtream.kt`/`Net.kt` listas M3U e Xtream
  - `Epg.kt`, `Futebol.kt`, `Trending.kt`, `Logos.kt`, `Sync.kt`, `Updater.kt`, `LocalServer.kt`
    (alguns dormentes nesta versão)
- As cores do tema ficam centralizadas em `UI.kt` e derivam do acento escolhido pelo usuário.

## Compilar e publicar
- Não há build local: a compilação roda no GitHub Actions (`.github/workflows/build.yml`).
  Push em `app/**` ou nos arquivos gradle gera o APK na release `tv-latest`.
- `versionCode` sobe sozinho no CI (`1000 + número da execução`); não editar à mão.
- O arquivo `min_version` no repositório controla a versão mínima aceita pela atualização
  automática; mexer nele só quando pedido.

## Regras do projeto
- Comentários e textos de interface em português do Brasil.
- Sempre adicionar comentários explicativos em funções de autenticação (regra geral do usuário).
- O app é feito para controle remoto de TV: toda tela nova precisa funcionar só com D-pad
  (foco visível, sem depender de toque).
- Segredos (`TMDB_KEY`, senha do keystore de release) ficam em GitHub Secrets, nunca no código.
  A chave em `signing/ntgree.jks` é pública de propósito (app pessoal, para atualizar por cima).
- Não fazer commit nem push sem o usuário pedir.
