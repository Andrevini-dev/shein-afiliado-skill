# Preparar o aparelho

## iPhone (ex.: iPhone 14 Plus)
- Quem toca na tela é o **agente** (Codex ou outro), com a ferramenta de controle do iPhone que ele já usa nesta máquina. A skill não instala nada no iPhone.
- O app **SHEIN** precisa estar instalado e logado na **conta de afiliada** (login feito pela pessoa).
- Conferir uma vez: **Perfil → Affiliate & Creator** abre a Central (saldo, "Pesquisar produtos", botões **Ganhe**). Se abrir "Afiliado Novo / Registre-se / Entrar", a conta logada não é afiliada.
- Para o texto copiado chegar ao computador:
  - Mac com o mesmo Apple ID: a Área de Transferência Universal leva o "Copiar" do iPhone para o Mac → `registrar.py --clipboard`.
  - Sem isso: o agente lê o texto do painel na tela e salva num arquivo → `registrar.py --arquivo texto.txt`.
- Deixar o iPhone desbloqueado e sem bloqueio automático durante a mineração.

## Android no cabo USB (modo automático)
1. Configurações → Sobre o telefone → tocar 7× em "Número da versão" (Xiaomi: "Versão do MIUI/HyperOS").
2. Opções do desenvolvedor → **Depuração USB** (Xiaomi: também **Depuração USB (configurações de segurança)**, senão `input tap` não funciona).
3. Cabo de dados → aceitar "Permitir depuração USB?" (Sempre permitir).
4. Desligar o bloqueio automático de tela durante a mineração.
5. `adb`: Platform Tools oficial <https://dl.google.com/android/repository/platform-tools-latest-windows.zip> (Windows) ou `brew install android-platform-tools` (Mac).

Testado em 12/11/2026: Xiaomi 22101320G (1080x2400), Android 13, SHEIN 15.5.8 — ~25–30 s por link.
