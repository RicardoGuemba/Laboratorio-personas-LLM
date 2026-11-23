# Laboratório de personas com LLMs locais

Simulador de debate e conversa casual com personas configuráveis: qualquer LLM do **LM Studio** pode ser debatedor, com **Perfis** (personalidade) e **Skills** editáveis na interface.

**Primeira experiência.** Abra `Abrir Laboratorio Personas LLM.app`. Configure dois participantes com perspectivas diferentes, escolha o modo de debate e acompanhe a troca de argumentos por turnos.

**Arquivos de referência**

- [docs/SISTEMA.md](docs/SISTEMA.md)
- [docs/DEBATE_RULES.md](docs/DEBATE_RULES.md)
- [config.yaml](config.yaml)

## Início rápido (duplo clique)

1. Instale **Python 3.11+**, **Node 18+** e **LM Studio**
2. No LM Studio: Start Server na porta **1234**, JIT + Auto-Evict ON
3. Primeira vez — instale dependências:

```bash
bash scripts/install_deps.sh
bash scripts/preparar-app.sh
```

4. **Duplo clique em `Abrir Laboratorio Personas LLM.app`**

   Alternativa técnica: `Laboratorio-Personas-LLM.app` (botão direito → Abrir na 1ª vez)

   Se nada abrir:

```bash
bash scripts/preparar-app.sh
```

Para encerrar: duplo clique em `Laboratorio-Personas-LLM-Stop.command`

### Problemas comuns

- **“Servidor não iniciou a tempo”** — porta 8765 em uso. Rode `Laboratorio-Personas-LLM-Stop.command` e tente de novo.
- **LM Studio offline** — abra o LM Studio e inicie o servidor na porta 1234.
- **Logs:** `.lab-personas-llm.log` (servidor) e `.lab-personas-llm-launcher.log` (launcher)

## Modos

| Modo | Descrição |
|------|-----------|
| **Debate formal** | Tema definido, 12 turnos, placar, avanço manual |
| **Conversa casual** | Tema automático, papo infinito, pare quando quiser |

Ver [`docs/SISTEMA.md`](docs/SISTEMA.md) para uso completo e arquitetura.

## Desenvolvimento

Terminal 1 — backend:

```bash
cd /caminho/Laboratório-personas-LLM
source .venv/bin/activate
PYTHONPATH=. uvicorn backend.main:app --reload --host 127.0.0.1 --port 8765
```

Terminal 2 — frontend:

```bash
cd frontend && npm run dev
```

Abra `http://localhost:5173` (proxy para API).

## Build manual

```bash
bash scripts/build.sh
PYTHONPATH=. uvicorn backend.main:app --host 127.0.0.1 --port 8765
```

## Testes

```bash
pytest tests/                    # unitários (mock LM Studio)
python tests/integration/test_debate_e2e.py   # E2E debate (servidor + LM live)
python tests/integration/test_casual_e2e.py   # E2E conversa casual
```

## Configuração

Edite [`config.yaml`](config.yaml) — modelos LM Studio, timeouts, regras de debate/casual.

## Documentação

- [`docs/SISTEMA.md`](docs/SISTEMA.md) — **uso e arquitetura**
- [`docs/DEBATE_RULES.md`](docs/DEBATE_RULES.md) — fases, personas, placar
- [`docs/CONTEXT.md`](docs/CONTEXT.md) — índice do projeto

## Estrutura

```
Abrir Laboratorio Personas LLM.app/  ← duplo clique (entrada)
Laboratorio-Personas-LLM.app/        ← bundle interno do servidor
backend/                             ← FastAPI + static build
frontend/src/                        ← React + avatares SVG
personas/                            ← perfis e skills (Markdown)
config.yaml
scripts/                             ← install, build, preparar-app
tests/                               ← pytest + E2E integration
```
