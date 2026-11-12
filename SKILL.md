---
name: shein-afiliado-minerador
description: Gera links de afiliado SHEIN em lote (mensagem pronta no padrão do app + link onelink ..._KOC-C) controlando o app SHEIN num Android pelo adb — celular no USB ou emulador no PC (BlueStacks, o caminho para quem tem iPhone). Use quando a pessoa pedir para "minerar/gerar links de afiliado da SHEIN", "achadinhos SHEIN", "links pro grupo", "N links de vestido/blusa", retomar uma mineração ou configurar o minerador.
---

# Minerador de links de afiliado SHEIN

O Programa de Afiliados da SHEIN só existe dentro do **app** (o site diz "acessível apenas em dispositivos móveis"; não há API pública). Esta skill usa `scripts/minerador.py` (Python puro + `adb`) para abrir a Central de Afiliados, pesquisar termos, tocar em **Ganhe** em cada produto e ler o texto de compartilhamento que já traz o link de afiliado.

## Regras que não mudam
- **Nunca** digitar, pedir ou guardar senha. Login (SHEIN, Google, Play Store) é sempre a pessoa que faz, na tela do aparelho. Avise numa linha ("faça login no BlueStacks") e espere.
- **Nunca** burlar CAPTCHA/verificação de robô. O script para, apita e confere a cada 30 s; diga à pessoa para resolver na tela.
- **Ritmo humano**: não reduza `pausa_min_s`/`pausa_max_s` (3–8 s) nem os descansos; uma requisição por vez, sem paralelizar.
- **ID da afiliada**: todo link é conferido pelo `koc_id` dentro da página do onelink (o `ismg_ol=..._KOC-C` muda a cada link e NÃO identifica a conta). Se o script parar com `PAREI: ... outro ID de afiliada` ou `NÃO é de afiliada`, pare tudo e avise.
- **Segurança de toque**: o script só toca se a SHEIN estiver em primeiro plano. Não desligue isso. Se aparecer `outro app está na frente`, investigue (ligação, notificação) antes de seguir; nunca toque em outro app.
- Nada é publicado/enviado a terceiros sem pedido explícito da pessoa.

## Passo a passo para o agente

1. **Aparelho**
   - Rode `adb devices -l`. Precisa aparecer um aparelho `device`.
   - iPhone → não dá para controlar pelo Windows. Use emulador: BlueStacks em **Retrato 1080x1920**, **ADB ligado** (Configurações → Avançado), SHEIN instalada pela Play Store e logada na conta de afiliada. Endereço típico `127.0.0.1:5555` (vai em `"emulador"` no config). Detalhes em `references/aparelho.md`.
   - Android no USB → Depuração USB ligada (Xiaomi: também "Depuração USB (configurações de segurança)").
2. **Config** (pasta de trabalho da pessoa, ex.: `~/shein-links`):
   - Copie `scripts/config.exemplo.json` para `<pasta>/config.json` e ajuste `buscas`, `quantidade_alvo`, `adb`, `emulador`, `koc_id_afiliada` (vazio na 1ª vez: o script mostra o ID detectado; confirme com a pessoa e grave).
   - `formato_saida`: `"mensagem"` (padrão do app: "Não perca esta oferta..." + 💰Preço + 🛒título + 🎁cupom + link) ou `"lista"` (`01 • título` + link).
3. **Teste curto**: rode com `quantidade_alvo` 10, confira os links e o `koc_id`, mostre à pessoa.
4. **Rodada completa** (em segundo plano, com log):
   ```bash
   python scripts/minerador.py --pasta <pasta> > <pasta>/saida/log.txt 2>&1
   ```
   Vigie o log por `CAPTCHA|pediu login|PAREI|NÃO é de afiliada|outro app|Traceback|Pronto`. Progresso a cada 50 links.
5. **Retomar**: é só rodar de novo; o progresso fica em `<pasta>/saida/progresso.json` (sem duplicados por `goods_id`).
6. **Entrega**: `<pasta>/saida/links_shein_AAAA-MM-DD.txt` (geral) e `links_<busca>.txt` (por termo). `python scripts/minerador.py txt --pasta <pasta>` regera os .txt.

## Ritmo esperado
~25–30 s por link (pausas incluídas) → 100 links ≈ 50 min; 300 ≈ 2h30.

## Quando algo dá errado
Veja `references/solucao-de-problemas.md` (mensagens do script, telas do app, layout da Central).
