# CLAUDE.md — Instruções para o Assistente IA

> Este ficheiro define como o Claude (ou qualquer assistente IA) deve trabalhar neste repositório. Deve ser lido no início de cada sessão de trabalho.

---

## 🎯 Contexto do Projeto

Este repositório (`GTFS_UserGuide/netex-pt-formacao/`) é um **curso educativo aberto em português sobre NeTEx** (Network Timetable Exchange), dirigido a técnicos de operadores de transporte público portugueses.

**Documentos de referência obrigatória antes de qualquer implementação:**
- `PLANO_IMPLEMENTACAO.md` — plano de tasks por fase (roadmap)
- `CONTRIBUTING.md` — convenções, regras de citação
- `referencias/README.md` — registo central de referências
- `netex_pt_formacao_plano.md` (na pasta pai) — plano original detalhado

---

## 📋 Regras de Implementação

### 1. Seguir o plano de tasks

- Consultar **sempre** o `PLANO_IMPLEMENTACAO.md` antes de iniciar trabalho
- Seguir a ordem das fases (1 → 2 → 3 → 4)
- Dentro de cada fase, respeitar a ordem dos módulos (0 → 1 → 2 → ...)
- Não saltar módulos — cada um constrói sobre o anterior

### 2. Citações obrigatórias (anti-plágio)

- **Toda informação técnica** deve incluir referência à fonte original
- Usar os IDs do registo central (`referencias/README.md`): `[STD-01]`, `[REG-01]`, `[PT-01]`, etc.
- Texto adaptado do projeto francês → indicar **"Adaptado de [PROJ-01]"**
- Dados de operadores reais → creditar fonte e URL
- Cada módulo **DEVE** terminar com secção `## Referências` listando todos os IDs citados
- **Se uma nova fonte for usada**, adicioná-la primeiro ao `referencias/README.md` antes de citar no módulo

### 3. Estrutura de cada módulo

Ao criar ou atualizar conteúdo de um módulo, seguir esta estrutura:

```markdown
# Módulo X — Título

> *Tempo estimado: X semanas*

## Objetivos de Aprendizagem
(lista de competências adquiridas)

## Conteúdo
(secções numeradas do módulo)

## Exemplos
(links para ficheiros XML na pasta `exemplos/`)

## Exercícios
(links para `exercicios/exercicio_XX.md`)

## Referências
[STD-01] CEN. "NeTEx – Network Timetable Exchange." ...
[REG-01] Regulamento Delegado (UE) 2024/490 ...
```

### 4. Ficheiros XML de exemplo

- **Sempre** comentados extensivamente com `<!-- ... -->` em português
- Comentários devem explicar **o porquê**, não apenas o que
- Válidos contra o schema NeTEx XSD
- Nomeados com prefixo do módulo: `02_01_linha_simples.xml`
- Usar dados do feed GTFS exemplo (`exemplos/gtfs_feed (1).zip`) como base para a ponte GTFS → NeTEx

### 5. Ponte GTFS → NeTEx

- Para **cada conceito NeTEx** apresentado, mostrar o equivalente GTFS
- Usar tabelas comparativas lado-a-lado
- Explicar o que o NeTEx adiciona ou modifica em relação ao GTFS
- Esta é a **decisão pedagógica mais importante** do curso — o público-alvo já conhece GTFS

### 6. Idioma e tom

- Todo o conteúdo em **português de Portugal** (PT-PT)
- Tom pedagógico, claro e prático — evitar linguagem excessivamente académica
- Termos técnicos NeTEx mantidos em inglês (ex: `VersionFrame`, `StopPlace`)
- Usar exemplos de contexto português (Carris, STCP, Metro de Lisboa, CP)

### 7. Atualizações ao plano

- Ao completar uma task, atualizar o `PLANO_IMPLEMENTACAO.md`:
  - Marcar a task como concluída
  - Adicionar notas relevantes se aplicável
- Ao completar todos os módulos de uma fase, atualizar:
  - `CHANGELOG.md` com a nova versão
  - `README.md` principal — mapa de módulos (📋 → 🚧 → ✅)

---

## 🔧 Convenções Técnicas

| Aspeto | Regra |
|--------|-------|
| Encoding | UTF-8 sem BOM |
| Nomes de ficheiro | `snake_case` com prefixo numérico do módulo |
| Comentários XML | Em português |
| Line endings | LF (Unix) |
| Markdown | GitHub Flavored Markdown (GFM) |
| Diagramas | Mermaid quando possível |

---

## ⚠️ O que NÃO fazer

- ❌ Criar conteúdo sem citar fontes
- ❌ Saltar fases ou módulos do plano
- ❌ Usar exemplos genéricos não-portugueses
- ❌ Escrever XML sem comentários explicativos
- ❌ Apresentar NeTEx sem a ponte comparativa com GTFS
- ❌ Modificar ficheiros fora de `GTFS_UserGuide/netex-pt-formacao/` sem autorização
- ❌ Ignorar o registo central de referências
