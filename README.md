# Skill: minerador de links de afiliado SHEIN

Skill no formato **Agent Skills** (`SKILL.md` + `scripts/` + `references/`) para usar com **Claude Code**, **Codex** e outros agentes que leem skills ou `AGENTS.md`.
Com ela instalada, é só pedir ao agente: *"gera 100 links de afiliado da SHEIN de vestido e blusa"*. No **iPhone**, o agente toca no app com a ferramenta de controle do iPhone que já tem e o `registrar.py` confere e organiza os links; no **Android**, o `minerador.py` faz tudo pelo cabo USB. No fim saem os `.txt` prontos para o agendador do WhatsApp.

O programa em si também existe sozinho (para rodar sem agente): <https://github.com/Andrevini-dev/shein-afiliado-minerador>

## Instalar

**Claude Code** (todas as pastas/projetos):
```bash
git clone https://github.com/Andrevini-dev/shein-afiliado-skill.git ~/.claude/skills/shein-afiliado-minerador
```
No Windows (Prompt de Comando):
```bat
git clone https://github.com/Andrevini-dev/shein-afiliado-skill.git "%USERPROFILE%\.claude\skills\shein-afiliado-minerador"
```

**Codex** (CLI/app):
```bash
git clone https://github.com/Andrevini-dev/shein-afiliado-skill.git ~/.codex/skills/shein-afiliado-minerador
```

**Outros agentes** (Cursor, Gemini CLI, etc.): clone na pasta do projeto e aponte o agente para o `AGENTS.md` deste repositório.

## O que precisa no PC
- Python 3.10+ (só biblioteca padrão; nada para instalar com pip).
- O app SHEIN logado na conta de **afiliada**:
  - **iPhone:** o agente precisa conseguir controlar o iPhone (a ferramenta que ele já usa). Detalhes em `references/aparelho.md`.
  - **Android:** celular no USB com Depuração USB e o `adb` (Android Platform Tools).

## Arquivos
| Arquivo | Para quê |
|---|---|
| `SKILL.md` | instruções que o agente segue (regras, passo a passo, ritmo) |
| `AGENTS.md` | atalho para agentes que leem `AGENTS.md` |
| `scripts/registrar.py` | iPhone/agente: recebe o texto copiado, confere a afiliada, tira repetidos e gera os .txt |
| `scripts/minerador.py` | Android: minerador automático (Python puro + adb) |
| `scripts/config.exemplo.json` | modelo de configuração |
| `references/aparelho.md` | preparar o iPhone (agente) ou o celular Android |
| `references/solucao-de-problemas.md` | mapa do app, mensagens do script e lições |

## Avisos
- Uso pessoal, ritmo baixo (pausas de 3–8 s e descansos), respeitando os termos do Programa de Afiliados SHEIN.
- O agente **nunca** digita senha nem burla CAPTCHA: login e verificações são sempre feitos pela pessoa, na tela.
- Config, progresso e links ficam na pasta de trabalho da pessoa (`--pasta`), fora deste repositório.
