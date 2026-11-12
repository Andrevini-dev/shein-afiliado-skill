# Preparar o aparelho

## iPhone (ex.: iPhone 14 Plus) → emulador Android no PC
A Apple não permite controlar o iPhone a partir do Windows (sem Mac não há WebDriverAgent/Appium para iOS). A solução é um Android virtual:

1. Instalar o BlueStacks pelo site oficial (<https://www.bluestacks.com>).
2. Configurações do BlueStacks:
   - **Tela/Display** → Orientação **Retrato (Portrait)**, resolução **1080 x 1920**. Em paisagem o script avisa ("tela está deitada") e o layout da Central muda.
   - **Avançado/Advanced** → **Android Debug Bridge (ADB)** ligado. Anote o endereço (ex.: `127.0.0.1:5555`).
3. Play Store → login Google (a pessoa faz) → instalar **SHEIN**.
4. Abrir a SHEIN → login na **conta de afiliada** (a pessoa faz).
5. Conferir: **Perfil → Affiliate & Creator** abre a Central (saldo, "Pesquisar produtos", botões **Ganhe**). Se abrir "Afiliado Novo / Registre-se / Entrar", a conta logada não é afiliada.
6. `config.json`: `"emulador": "127.0.0.1:5555"`.

Testado em 12/11/2026: BlueStacks 5 (Android 9, Pie64) roda o app SHEIN 15.5.8 sem bloqueio de emulador; aceita APK arm64. Outros emuladores (MuMu `127.0.0.1:7555`, Nox `127.0.0.1:62001`, MEmu `127.0.0.1:21503`, LDPlayer `emulator-5554`) devem funcionar igual (não testados). Com `"emulador": ""` o script tenta essas portas sozinho.

## Android no cabo USB
1. Configurações → Sobre o telefone → tocar 7× em "Número da versão" (Xiaomi: "Versão do MIUI/HyperOS").
2. Opções do desenvolvedor → **Depuração USB** (Xiaomi: também **Depuração USB (configurações de segurança)**, senão `input tap` não funciona).
3. Cabo de dados → aceitar "Permitir depuração USB?" (Sempre permitir).
4. Desligar o bloqueio automático de tela durante a mineração.

Testado em 12/11/2026: Xiaomi 22101320G (1080x2400), Android 13, SHEIN 15.5.8.

## adb no Windows
Platform Tools oficial: <https://dl.google.com/android/repository/platform-tools-latest-windows.zip> → extrair em `C:\platform-tools` → `"adb": "C:\\platform-tools\\adb.exe"`.
