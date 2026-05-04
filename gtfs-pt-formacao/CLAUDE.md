# CLAUDE.md — Instruções para o Assistente IA

> Este ficheiro define como o Claude (ou qualquer assistente IA) deve trabalhar neste repositório. Deve ser lido no início de cada sessão de trabalho.

---

## Contexto do Projeto

Este repositório (`GTFS_UserGuide/gtfs-pt-formacao/`) é um **curso educativo aberto em português sobre GTFS** (General Transit Feed Specification), dirigido a técnicos de operadores de transporte público, profissionais de sistemas de informação de mobilidade e estudantes de engenharia de transportes.

**Documentos de referência obrigatória antes de qualquer implementação:**
- `CONTRIBUTING.md` — convenções, regras de citação
- `referencias/README.md` — registo central de referências
- `../README.md` (na pasta pai) — documentação GTFS original em inglês

---

## Regras de Implementação

### 1. Seguir a ordem dos módulos

- Os módulos são sequenciais (0 → 1 → 2 → ... → 8)
- Cada módulo constrói sobre o anterior
- Não saltar módulos

### 2. Citações obrigatórias (anti-plágio)

- **Toda informação técnica** deve incluir referência à fonte original
- Usar os IDs do registo central (`referencias/README.md`): `[STD-01]`, `[BP-01]`, `[DATA-01]`, etc.
- Dados de operadores reais → creditar fonte
- Cada módulo **DEVE** terminar com secção `## Referências` listando todos os IDs citados
- **Se uma nova fonte for usada**, adicioná-la primeiro ao `referencias/README.md` antes de citar no módulo

### 3. Estrutura de cada módulo

Ao criar ou atualizar conteúdo de um módulo, seguir esta estrutura:

```markdown
# Módulo X — Título

## Objetivos de Aprendizagem
(lista de competências adquiridas)

## Conteúdo
(secções numeradas do módulo)

## Exemplos
(links para ficheiros CSV/TXT na pasta `exemplos/`)

## Exercícios
(links para `exercicios/exercicio_XX.md`)

## Referências
[STD-01] MobilityData. "GTFS Schedule Reference." ...
[BP-01] MobilityData. "GTFS Schedule Best Practices." ...
```

### 4. Ficheiros de exemplo

- Ficheiros CSV/TXT seguindo o formato GTFS (cabeçalhos corretos, vírgula como separador)
- O conteúdo explicativo (comentários, anotações) vai no README.md do módulo, não nos ficheiros CSV
- Nomeados com prefixo do módulo: `02_01_agency_stcp.txt`
- Usar dados do feed GTFS da STCP Porto como base

### 5. Ponte NeTEx (opcional)

- Para conceitos onde existe um equivalente NeTEx relevante, mencioná-lo brevemente
- Esta ponte é **opcional** (ao contrário do curso NeTEx, onde a ponte GTFS é obrigatória)
- A comparação detalhada está em `recursos/comparacao_gtfs_netex.md`

### 6. Idioma e tom

- Todo o conteúdo em **português de Portugal** (PT-PT)
- Tom pedagógico, claro e prático — evitar linguagem excessivamente académica
- Termos técnicos GTFS mantidos em inglês (ex: `stop_id`, `route_type`, `trip_id`)
- Usar exemplos de contexto português (STCP, Carris, Metro do Porto, CP)

### 7. Atualizações ao plano

- Ao completar um módulo, atualizar:
  - `CHANGELOG.md` com a nova versão
  - `README.md` principal — mapa de módulos (Planeado → Em Progresso → Completo)

---

## Convenções Técnicas

| Aspeto | Regra |
|--------|-------|
| Encoding | UTF-8 sem BOM |
| Nomes de ficheiro | `snake_case` com prefixo numérico do módulo |
| Ficheiros CSV | Cabeçalhos na primeira linha, vírgula como separador |
| Line endings | LF (Unix) para Markdown, CRLF para CSV (convenção GTFS) |
| Markdown | GitHub Flavored Markdown (GFM) |
| Diagramas | ASCII ou Mermaid quando possível |

---

## O que NÃO fazer

- Criar conteúdo sem citar fontes
- Saltar módulos
- Usar exemplos genéricos não-portugueses quando existem dados reais disponíveis
- Apresentar GTFS sem explicar o porquê de cada campo e decisão de design
- Modificar ficheiros fora de `GTFS_UserGuide/gtfs-pt-formacao/` sem autorização
- Ignorar o registo central de referências
