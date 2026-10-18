# skills-bp

Base de skills reutilizáveis para novos repositórios, independente do modelo, agente ou harness.

O conteúdo é versionado no GitHub. A instalação é local a cada projeto; não exige configuração global nem um runtime de agente específico.

## Catálogo

| Skill | Objetivo | Origem | Licença |
| --- | --- | --- | --- |
| `find-skills` | Descobrir e instalar skills | `vercel-labs/skills`, `skills/find-skills` | MIT |
| `agent-browser` | Orientar automação de navegador com o CLI agent-browser | `vercel-labs/agent-browser`, `skills/agent-browser` | Apache-2.0 |
| `mcp-builder` | Construir e avaliar servidores MCP | `anthropics/skills`, `skills/mcp-builder` | Apache-2.0 |
| `python-performance-optimization` | Medir e otimizar desempenho de Python | `sickn33/agentic-awesome-skills`, `skills/python-performance-optimization` | MIT |

O primeiro link fornecido é o repositório do Skills CLI; a skill pública correspondente é `find-skills`. Não importamos os workflows internos de triagem desse repositório nem todas as skills das outras coleções.

## Bootstrap de um projeto

### Opção 1: Skills CLI

Pré-requisitos do caminho testado: Git e Node.js >= 22.20.0. Execute **na raiz do projeto de destino**:

```sh
# Inspecionar o catálogo sem instalar.
npx --yes skills@1.7.2 add PauloSanches/skills-bp --list

# Instalar as quatro skills apenas neste projeto, no diretório comum .agents/skills.
npx --yes skills@1.7.2 add PauloSanches/skills-bp --agent universal --skill '*' --copy --yes
```

Para instalar apenas uma seleção:

```sh
npx --yes skills@1.7.2 add PauloSanches/skills-bp --agent universal --skill mcp-builder python-performance-optimization --copy --yes
```

`universal` designa o diretório partilhado `.agents/skills`; **não garante descoberta automática em todos os harnesses**. Se o harness exigir outra localização, selecione o adaptador adequado com `--agent` ou indique explicitamente os ficheiros ao agente. Nenhuma destas operações instala globalmente.

Reveja os ficheiros existentes antes da instalação: o Skills CLI pode substituir skills com os mesmos nomes. O template `templates/AGENTS.md` é opcional; integre-o manualmente no `AGENTS.md` existente, sem o sobrescrever.

### Opção 2: Sem Skills CLI

Obtenha uma revisão deste repositório e copie apenas os diretórios pretendidos de `skills/` para o projeto, preservando os ficheiros de apoio e de licença. Use `.agents/skills/` como convenção, ou outra localização explícita. Peça ao agente para ler o `SKILL.md` relevante e os recursos referenciados. Esta opção não depende de Node.js nem de suporte nativo a skills.

## Portabilidade e dependências

- O núcleo é constituído por ficheiros, não por uma API de modelo ou runtime de agente.
- Os originais upstream são preservados integralmente. Podem conter nomes de ferramentas (`Bash`, `WebFetch`), metadados específicos de harnesses (`allowed-tools`, `hidden`) ou exemplos de fornecedores. Esses elementos não são uma API universal: use capacidades equivalentes e as regras/permissões do ambiente em uso.
- Uma skill é conteúdo instrucional, não uma instalação das ferramentas que descreve. Não execute automaticamente scripts, instalações globais ou operações externas sugeridas nos documentos.
- `agent-browser/SKILL.md` é o ponto de descoberta oficial; o guia operacional vem de `agent-browser skills get core`, servido pela versão instalada do CLI. Usá-lo exige instalar separadamente o CLI e um browser compatível. O bootstrap desta base não instala nem testa esse runtime.
- `mcp-builder` preserva `reference/`, os scripts de avaliação e os requisitos Python. O bootstrap não instala essas dependências nem executa avaliações que precisem de credenciais/modelos.
- A skill de desempenho Python inclui `resources/implementation-playbook.md`. Profilers e restantes dependências são selecionados conforme o projeto.

## Proveniência, integridade e atualizações

`sources.lock.json` regista o commit upstream, o caminho original, a licença e os hashes de **cada ficheiro importado**, incluindo licenças e notices. É o manifesto desta base; não é o `skills-lock.json` gerado pelo Skills CLI num projeto consumidor.

As cópias vendorizadas não seguem `main` automaticamente. Para atualizar:

1. Escolha uma revisão upstream e reveja todas as diferenças e licenças.
2. Atualize a skill completa, incluindo recursos e notices.
3. Atualize o manifesto de proveniência e hashes.
4. Valide o catálogo e teste uma instalação num projeto descartável.
5. Publique a alteração e reveja separadamente o update nos projetos consumidores.

Para reproduzir exatamente uma instalação, use um checkout deste repositório num commit conhecido e passe o caminho local ao mesmo comando `skills add`, em vez de resolver o ramo remoto novamente.

As quatro skills são distribuídas sob as licenças upstream indicadas em `THIRD_PARTY.md` e nos respetivos diretórios. A validação da distribuição não equivale a uma auditoria integral nem a testar todas as funcionalidades dos runtimes descritos.
