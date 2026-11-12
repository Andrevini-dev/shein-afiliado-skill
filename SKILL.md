---
name: shein-afiliado-minerador
description: Gera links de afiliado SHEIN em lote (mensagem pronta no padrão do app + link onelink ..._KOC-C) operando o app SHEIN no celular da pessoa — no iPhone, o próprio agente faz os toques com a ferramenta de controle do iPhone que ele já tem e o scripts/registrar.py confere e organiza; no Android, o scripts/minerador.py faz tudo pelo adb. Use quando a pessoa pedir para "minerar/gerar links de afiliado da SHEIN", "achadinhos SHEIN", "links pro grupo/agendador do zap", "N links de vestido/blusa", ou retomar uma mineração.
---

# Minerador de links de afiliado SHEIN

O Programa de Afiliados da SHEIN só existe dentro do **app** (o site diz "acessível apenas em dispositivos móveis"; não há API pública). O link de afiliado sai pronto no painel **Compartilhe para ganhar** de cada produto.

## Regras que não mudam
- **Nunca** digitar, pedir ou guardar senha. Login é sempre a pessoa que faz, na tela do celular. Avise numa linha e espere.
- **Nunca** burlar CAPTCHA/verificação de robô. Peça para a pessoa resolver, confira a cada 30 s e continue.
- **Ritmo humano:** 3–8 s aleatórios entre produtos e um descanso de 1–2 min a cada 25 links. Um produto por vez, nada em paralelo.
- **ID da afiliada:** todo link é conferido pelo `koc_id` dentro da página do onelink (o `ismg_ol=..._KOC-C` muda a cada link e NÃO identifica a conta). Se aparecer `PAREI` (outra afiliada) ou a Central mostrar a inscrição "Afiliado Novo / Registre-se", pare tudo e avise.
- **Só mexer no app SHEIN.** Se uma ligação, notificação ou outro app tomar a tela, não toque em nada: espere e volte para a SHEIN.
- Não enviar links para ninguém nem publicar nada sem pedido explícito da pessoa.

## Pasta de trabalho
Use uma pasta da pessoa, fora da skill (ex.: `~/shein-links`). Na 1ª vez, copie `scripts/config.exemplo.json` para `<pasta>/config.json` e ajuste `buscas`, `quantidade_alvo` e `formato_saida` (`"mensagem"` = padrão do app; `"lista"` = `01 • título` + link). `koc_id_afiliada` fica vazio na 1ª vez: o script mostra o ID detectado; confirme com a pessoa e grave.

## iPhone — o agente opera o app (caminho principal)
Use a ferramenta de controle do iPhone que você (agente) já tem nesta máquina. Fluxo por termo de busca:

1. Abrir o app **SHEIN** → aba **Perfil** → **Affiliate & Creator** (alternativa: barra de busca da aba Loja → `afiliado`). Deve abrir a Central: título **Programa de Afiliados**, saldo, **Pesquisar produtos**.
2. **Pesquisar produtos** → digitar o termo (ex.: `vestido`) → Pesquisa.
3. Para cada produto da lista (de cima para baixo, rolando quando acabar):
   1. Consultar `python scripts/registrar.py --pasta <pasta> --status` de vez em quando e pular títulos já registrados.
   2. Tocar em **Ganhe** → painel **Compartilhe para ganhar**.
   3. Tocar em **Copiar** (copia o texto inteiro: "Não perca esta oferta…", 💰Preço, 🛒título, 🎁cupom e o link) e fechar o painel pelo **X** (o "voltar" sai da busca).
   4. Registrar: `python scripts/registrar.py --pasta <pasta> --categoria <termo> --clipboard`
      - Se o clipboard do celular não chegar no computador, leia o texto do painel na tela e passe por arquivo: `--arquivo texto.txt` (ou pela entrada padrão). Pode juntar vários produtos num texto só.
   5. Resposta: `NOVO` (ok), `REPETIDO` (siga), `PAREI` (código 2: outra afiliada → pare e avise).
   6. Pausa aleatória de 3–8 s.
4. Ao bater a meta do termo (`quantidade_alvo` ÷ número de buscas), voltar à Central e ir para o próximo termo.
5. Comece com **10 links de teste**, mostre à pessoa, e só então siga até a meta. Informe o progresso a cada 50.

## Android — automático pelo adb (alternativa)
Celular Android no USB com **Depuração USB** (Xiaomi: também "Depuração USB (configurações de segurança)"), app SHEIN logado na conta de afiliada:
```bash
python scripts/minerador.py --pasta <pasta> > <pasta>/saida/log.txt 2>&1
```
Vigie o log por `CAPTCHA|pediu login|PAREI|NÃO é de afiliada|outro app|Traceback|Pronto`. Rodar de novo continua de onde parou.

## Entrega
- `<pasta>/saida/links_shein_AAAA-MM-DD.txt` (todos, uma mensagem por produto, linha em branco entre elas) e `links_<termo>.txt`.
- Refazer os .txt: `python scripts/registrar.py --pasta <pasta> --txt`.
- Esse .txt é o que vai para o agendador de mensagens do WhatsApp (cada bloco = uma mensagem).

## Ritmo esperado
Android automático: ~25–30 s por link (100 ≈ 50 min). iPhone pelo agente: depende da ferramenta de controle; mantenha as pausas.

Mais detalhes: `references/aparelho.md` (preparar iPhone/Android) e `references/solucao-de-problemas.md` (mapa do app, mensagens e lições).
