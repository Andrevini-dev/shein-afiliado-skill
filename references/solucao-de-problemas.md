# Solução de problemas e mapa do app

## Mapa da Central de Afiliados (app SHEIN 15.5.x, pt-BR)
- Entrada 1: aba **Perfil** → item **Affiliate & Creator**.
- Entrada 2: buscar **afiliado** na barra de busca da aba **Loja**.
- Central (WebView `com.zzkko.uicomponent.WebViewActivity`): título **Programa de Afiliados**, saldo, "Código de Indicação", atalhos (Centro de Dados, Converter Link, Mais vendidos, Bônus, Meus produtos), **Pesquisar produtos**.
- Busca da Central: campo `EditText` + botão **Pesquisa**; cada produto tem título, preço, "Escolher" e botão **Ganhe**.
- **Ganhe** abre o painel **Compartilhe para ganhar** com o texto pronto:
  ```
  Não perca esta oferta grande na SHEIN! Economize muito agora!
  💰Preço[R$xx,xx] -20%
  🛒<título completo>
  🎁Cupom 50% OFF para todo Novo Usuário!
  https://onelink.shein.com/60/<código>?ismg_ol=<código>_01_KOC-C
  ```
  O texto é legível por `uiautomator dump` (não precisa de clipboard).
- Fechar o painel pelo **X** (acima do título, à direita). O botão **voltar** fecha o painel **e sai da busca**.
- A página do onelink (GET simples) traz `koc_id=<id da afiliada>` e `goods_id=<produto>` dentro do HTML.

## registrar.py (iPhone/agente)
| Resposta | Significado | Ação |
|---|---|---|
| `NOVO n • título` | link conferido e salvo | seguir |
| `REPETIDO` | produto já estava na lista | seguir para o próximo |
| `PAREI` (código 2) | link de outra afiliada | parar e avisar a pessoa |
| `NAO CONFERIDO` | sem internet ou link inválido | tentar de novo o mesmo produto |
| `Não achei nenhum link` | o texto copiado não tem o onelink | conferir se tocou em **Copiar** no painel certo |

## Mensagens do minerador.py (Android)
| Mensagem | Causa | Ação |
|---|---|---|
| `Nenhum celular pronto` | adb não vê o Android | cabo de dados, Depuração USB, aceitar o aviso |
| `AVISO: a tela está deitada` | celular em paisagem | deixar o celular em pé |
| `Esta conta da SHEIN NÃO é de afiliada` | Central abriu a página de inscrição | pessoa troca para a conta de afiliada |
| `PAREI: o link saiu com outro ID de afiliada` | koc_id diferente do config | parar e avisar; conferir conta |
| `outro app está na frente` | ligação, notificação, outro app | não tocar em nada; o script espera e reabre a SHEIN |
| `a busca não abriu direito 3 vezes` | termo sem resultados ou tela mudou | segue para o próximo termo |
| `CAPTCHA` / `pediu login` | verificação ou sessão expirada | pessoa resolve na tela; o script confere a cada 30 s |

## Lições do desenvolvimento (12/11/2026)
- Ler a tela 1× por lista e gerar todos os produtos visíveis em sequência baixou de ~80 s para ~25–30 s por link.
- `uiautomator dump` às vezes falha ("could not get idle state" em telas animadas como o Perfil): o script apaga o arquivo antes, tenta 3× e, se precisar, reabre o app na aba Loja.
- Uma ligação de WhatsApp tomou a frente do celular no meio da mineração; daí a trava "só toca com a SHEIN na frente".
