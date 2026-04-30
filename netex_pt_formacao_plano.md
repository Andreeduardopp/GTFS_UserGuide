# Plano de implementação — Curso NeTEx em português

> **Objetivo**: Criar o primeiro recurso educativo aberto em português sobre NeTEx (Network Timetable Exchange), equivalente ao projeto francês `etalab/netex-france-formation`, dirigido a técnicos de operadores de transporte público portugueses.
>
> **Formato**: Repositório GitHub com ficheiros Markdown + XML comentado.
>
> **Perfil**: Projeto individual, sem parceiros institucionais na fase inicial.
>
> **Duração estimada**: 4 a 6 meses para os primeiros 4 módulos publicados.

---

## Índice

1. [Contexto e justificação](#1-contexto-e-justificação)
2. [Fase 1 — Fundação](#2-fase-1--fundação-2-3-semanas)
3. [Fase 2 — Conteúdo base](#3-fase-2--conteúdo-base-6-8-semanas)
4. [Fase 3 — Conteúdo avançado](#4-fase-3--conteúdo-avançado-6-8-semanas)
5. [Fase 4 — Lançamento](#5-fase-4--lançamento-2-3-semanas)
6. [Fase 5 — Crescimento e sustentabilidade](#6-fase-5--crescimento-e-sustentabilidade)
7. [Estrutura do repositório](#7-estrutura-do-repositório)
8. [Recursos e referências](#8-recursos-e-referências)

---

## 1. Contexto e justificação

### O gap que este projeto preenche

Não existe atualmente nenhum recurso educativo aberto em português sobre NeTEx. Os recursos disponíveis são:

| Recurso | Língua | Formato | Limitação |
|---|---|---|---|
| `etalab/netex-france-formation` | Francês | Curso interativo | Não acessível a falantes de português |
| Data4PT workshops | Inglês | PDFs de workshops | Não estruturado como curso |
| Open Transport Data Switzerland | Inglês | Cookbook técnico | Muito técnico, sem pedagogia |
| IMT Portugal webinar 2021 | Português | Apresentações PDF | Não interativo, desatualizado |
| Documentação NeTEx oficial | Inglês | Spec técnica | Inacessível para iniciantes |

### Porquê agora

Portugal está a implementar o seu perfil nacional NeTEx (disponível em `ptprofiles.azurewebsites.net`) e o NAP nacional (`nap-portugal.imt-ip.pt`). O Regulamento Delegado EU 2024/490 exige que os operadores portugueses publiquem dados de transporte em NeTEx. Os técnicos dos operadores precisam de formação — e não existe nada em português.

### Público-alvo

Técnicos de operadores de transporte público portugueses com:

- Conhecimento básico de GTFS (já trabalham com ficheiros CSV de horários e paragens)
- Alguma familiaridade com XML
- Necessidade prática de implementar NeTEx para cumprir obrigações regulatórias europeias

---

## 2. Fase 1 — Fundação (2–3 semanas)

**Objetivo**: Antes de escrever uma linha de conteúdo, definir a estrutura, escolher as ferramentas e preparar o repositório. Um bom scaffolding evita reescritas dolorosas mais tarde.

### 2.1 Criação do repositório GitHub

- **Nome**: `netex-pt-formacao`
- **Licença**: Creative Commons CC BY 4.0 — padrão para recursos educativos abertos, a mesma usada pelo projeto francês
- **README inicial**: Em português, com o objetivo do projeto, o público-alvo, e um roadmap visual dos módulos previstos
- **Topics**: `netex`, `transmodel`, `transporte-publico`, `portugal`, `open-data`, `formacao`

### 2.2 Estrutura de pastas

```
netex-pt-formacao/
├── README.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── LICENSE
├── modulos/
│   ├── 00_antes_de_comecar/
│   │   ├── README.md
│   │   └── exemplos/
│   ├── 01_conceitos_base/
│   │   ├── README.md
│   │   ├── exemplos/
│   │   └── exercicios/
│   │       └── solucoes/
│   ├── 02_rede_transporte/
│   ├── 03_horarios_calendarios/
│   ├── 04_multimodalidade/
│   ├── 05_acessibilidade/
│   ├── 06_tarifas_introducao/
│   ├── 07_perfil_netex_portugal/
│   └── 08_ferramentas_praticas/
├── exemplos/
│   └── rede_exemplo_portugal/   ← dataset completo de referência
├── recursos/
│   ├── glossario.md
│   ├── comparacao_gtfs_netex.md
│   └── links_uteis.md
└── .github/
    └── ISSUE_TEMPLATE/
        ├── duvida.md
        └── sugestao_modulo.md
```

### 2.3 Escolha do caso de uso âncora

Usa uma rede de transporte real portuguesa como fio condutor de todos os exemplos XML. **Escolha recomendada: rede de autocarros da STCP (Porto)** — dados GTFS disponíveis publicamente, rede de tamanho médio, com Metro do Porto como modo complementar (permite módulo de multimodalidade).

Alternativamente, cria uma rede fictícia mas realista chamada **"Rede de Transportes de Exemplo — RTE"** com dados inspirados na realidade portuguesa. A vantagem é a liberdade para simplificar sem perder realismo.

### 2.4 Análise do projeto francês

Antes de começar a escrever, lê todos os ficheiros `.livemd` do repositório `etalab/netex-france-formation` e regista:

- O que adaptas diretamente (estrutura pedagógica, progressão de conceitos)
- O que reescreves para o contexto português (exemplos, regulação, perfil nacional)
- O que acrescentas (módulo sobre perfil PT, ponte explícita com GTFS)

### Entregável da Fase 1

> **Repositório GitHub criado** com estrutura de pastas, README, licença e nota de análise do projeto francês.

---

## 3. Fase 2 — Conteúdo base (6–8 semanas)

**Objetivo**: Produzir os primeiros 4 módulos — o núcleo do curso. São os mais difíceis de escrever porque estabelecem a linguagem, os conceitos e o ritmo pedagógico de todo o resto.

### 3.1 Módulo 0 — Antes de começar

Conteúdo:

- O que é o NeTEx e porque importa para os operadores portugueses
- Comparação rápida com GTFS: o que cada formato cobre e onde se sobrepõem
- O ecossistema: Transmodel → NeTEx → SIRI → NAP
- Regulação europeia relevante para Portugal (Regulamento 2024/490)
- Como usar este curso: pré-requisitos, ferramentas necessárias, como navegar os ficheiros XML

Tempo estimado: 1 semana

### 3.2 Módulo 1 — Conceitos base

Conteúdo:

- Entidades, atributos e relações no modelo NeTEx
- O que é um `VersionFrame` e como estrutura um ficheiro NeTEx
- Identificadores, referências e versionamento
- Como ler um ficheiro XML NeTEx pela primeira vez (anatomia de um ficheiro real)
- Exemplo prático: abrir e navegar um ficheiro NeTEx real da STCP ou equivalente

Ficheiro de exemplo: `modulos/01_conceitos_base/exemplos/01_frame_basico.xml`

Tempo estimado: 1,5 semanas

### 3.3 Módulo 2 — Rede de transporte

Conteúdo:

- `Line`, `Route`, `RoutePoint`, `ScheduledStopPoint`, `StopPlace`
- Como se relacionam estes elementos entre si
- Exemplo completo: modelar uma linha de autocarro portuguesa em NeTEx
- Comparação lado-a-lado com o equivalente GTFS (`routes.txt` + `stops.txt` + `shapes.txt`)
- Exercício: dado um extrato de `routes.txt` e `stops.txt` da STCP, converte para NeTEx

A ponte GTFS → NeTEx é especialmente importante neste módulo porque os técnicos do público-alvo já conhecem GTFS e precisam de ancorar os novos conceitos em algo familiar.

Tempo estimado: 2 semanas

### 3.4 Módulo 3 — Horários e calendários

Conteúdo:

- `ServiceJourney`, `TimetableFrame`, `DayType`, `DayTypeAssignment`
- Modelar variações de serviço: semana vs fim-de-semana, período escolar vs férias
- Exemplo: horários completos de uma linha com múltiplas variações de calendário
- Exercício: dado um horário em PDF de um operador português, produz o XML NeTEx

Tempo estimado: 2 semanas

### 3.5 Princípios pedagógicos

**Contexto português sempre**: cada exemplo XML usa dados reais ou realistas de operadores portugueses — Carris, Metro de Lisboa, STCP, CP. Nunca exemplos genéricos abstractos.

**Ponte GTFS → NeTEx**: para cada conceito NeTEx, mostra o equivalente GTFS e explica o que NeTEx adiciona ou modifica. Esta é a decisão pedagógica mais importante do curso.

**XML comentado**: todos os ficheiros de exemplo têm comentários `<!-- ... -->` extensivos que explicam cada elemento. O ficheiro XML é o documento de aprendizagem, não apenas um artefacto técnico.

**Exercícios com solução**: cada módulo termina com 1-2 exercícios práticos com solução na pasta `exercicios/solucoes/`.

### Entregável da Fase 2

> **4 módulos completos** (Módulos 0-3), ficheiros XML de exemplo comentados para cada módulo, exercícios com soluções.
>
> Versão: `v0.1.0-alpha`

---

## 4. Fase 3 — Conteúdo avançado (6–8 semanas)

**Objetivo**: Produzir os módulos mais especializados — os que diferenciam genuinamente este curso de qualquer documentação já existente em português e que são mais relevantes para técnicos portugueses no contexto regulatório atual.

### 4.1 Módulo 4 — Multimodalidade e StopPlace

Conteúdo:

- `StopPlace`, `Quay`, `BoardingPosition`, `PathLink`, `AccessSpace`
- Como modelar uma estação intermodal: entrada única, múltiplos modos
- Exemplo: Estação de Campanhã (comboio + metro + autocarro) ou Oriente
- Relação com o standard IFOPT (Identification of Fixed Objects in Public Transport)

Este módulo não existe de forma clara em nenhum recurso em português e é crítico para a implementação do perfil PT.

### 4.2 Módulo 5 — Acessibilidade

Conteúdo:

- `AccessibilityAssessment`, `LimitationStatusEnumeration`
- Como modelar: rampas, elevadores, informação tátil, piso baixo
- Alinhamento com o perfil europeu EPIAP (NeTEx Parte 6, publicada em 2024)
- Exemplo: paragem acessível vs paragem com limitações na rede da Carris

A acessibilidade é uma obrigação regulatória crescente e um dos casos de uso mais concretos do NeTEx para os operadores portugueses.

### 4.3 Módulo 6 — Tarifas (introdução)

Conteúdo:

- `FareFrame`, `TariffZone`, `FareProduct`, `PreassignedFareProduct`
- Exemplo: modelar o passe Andante do Porto ou o passe navegante em NeTEx
- Limitações: este módulo é uma introdução — tarifas complexas (NeTEx Parte 3) ficam para uma versão futura do curso

### 4.4 Módulo 7 — Perfil NeTEx Portugal

Conteúdo:

- O que é um perfil nacional e porque Portugal tem o seu
- Referência ao perfil em `ptprofiles.azurewebsites.net`
- Que campos são obrigatórios vs opcionais no contexto português
- Como enviar dados para o NAP nacional (`nap-portugal.imt-ip.pt`)
- Diferenças entre o perfil PT e o perfil francês (para quem vier do projeto francês)

**Este módulo é único em todo o ecossistema de recursos NeTEx** — não existe nada equivalente em nenhuma língua sobre o perfil português especificamente.

### 4.5 Módulo 8 — Ferramentas práticas

Conteúdo:

- Validação: `netex-validator` da Entur, XMLSpy, VS Code com extensão XML
- Conversão: ferramentas NeTEx ↔ GTFS disponíveis (e as suas limitações)
- Navegação: como encontrar o que precisas no schema XSD do NeTEx
- Checklist de implementação: o que verificar antes de submeter ao NAP
- Recursos para continuar a aprender

### Entregável da Fase 3

> **Módulos 4-8 completos**, dataset de exemplo completo cobrindo uma rede fictícia portuguesa do início ao fim, glossário PT-EN de termos NeTEx.
>
> Versão: `v0.2.0-beta`

---

## 5. Fase 4 — Lançamento (2–3 semanas)

**Objetivo**: Tornar o curso visível, útil e contribuível pela comunidade.

### 5.1 Antes de lançar

**Revisão técnica**: valida todos os ficheiros XML de exemplo com o validador oficial NeTEx. Garante que os exemplos são compatíveis com o perfil nacional português.

**Revisão de legibilidade**: pede a 1-2 técnicos de operadores que percorram o Módulo 0 e o Módulo 2 e dêem feedback. Ajusta antes de publicar.

**Documentação de contribuição**: cria o `CONTRIBUTING.md` com instruções claras sobre como reportar erros, propor novos módulos e submeter exemplos XML.

**Versionamento**: regista a versão `v0.1.0` no `CHANGELOG.md` com a lista de módulos incluídos.

### 5.2 Canais de divulgação

| Canal | Mensagem-chave | Prioridade |
|---|---|---|
| **IMT** (imt-ip.pt) | Primeiro curso em português sobre NeTEx, alinhado com o perfil PT | Alta |
| **NAP Portugal** (nap-portugal.imt-ip.pt) | Recurso para operadores que precisam de implementar NeTEx | Alta |
| **MobilityData Slack** (#general, #portugal) | Recurso aberto em português, primeiro do género | Alta |
| **LinkedIn** | Post público com contexto, impacto esperado, link | Média |
| **Transmodel CEN mailing list** | Notícia de alcance europeu — único curso em PT | Média |
| **GitHub Topics** | Aparece nas pesquisas de `netex`, `transmodel`, `portugal` | Baixa (automático) |

### 5.3 Template de mensagem para o IMT

> Assunto: Recurso educativo aberto sobre NeTEx em português
>
> Desenvolvemos o primeiro curso aberto em português sobre NeTEx, disponível em github.com/[user]/netex-pt-formacao. O curso é dirigido a técnicos de operadores de transporte público e inclui um módulo específico sobre o perfil nacional português e a ligação ao NAP. Gostaríamos de saber se o IMT teria interesse em referenciar ou colaborar com este recurso.

### Entregável da Fase 4

> **Repositório público com versão `v0.1.0`**: README completo, CONTRIBUTING.md, CHANGELOG.md, Módulos 0-3 validados, anúncio feito em pelo menos 3 canais.

---

## 6. Fase 5 — Crescimento e sustentabilidade

**Objetivo**: Transformar o projeto individual num recurso sustentável mantido pela comunidade.

### 6.1 Ciclo de atualização

- **Semestral**: revê o conteúdo para alinhar com novas versões do NeTEx (atualmente v2.1, com v3.0 em desenvolvimento) e com alterações ao perfil nacional português
- **Por evento**: atualiza sempre que o IMT publicar uma nova versão do perfil PT ou quando o NAP mudar os requisitos de submissão

### 6.2 Issues como motor de conteúdo

Encoraja utilizadores a abrir issues com dúvidas reais. As perguntas mais frequentes tornam-se FAQs ou novos exemplos XML. Usa labels como `duvida`, `sugestao`, `bug-xml`, `novo-modulo`.

### 6.3 Parceiros a contactar progressivamente

**Fase 1 (pós-lançamento)**: IMT — co-autoria ou endorsement formal. Propõe que validem o conteúdo e apareçam como parceiros institucionais.

**Fase 2 (3-6 meses)**: Operadores — Carris, STCP ou Metro de Lisboa para contribuírem com um módulo baseado na sua implementação real.

**Fase 3 (6-12 meses)**: Universidades — IST, FEUP, UM para integrarem o curso como material de apoio em unidades curriculares de Engenharia de Transportes.

### 6.4 Riscos e mitigações

| Risco | Probabilidade | Mitigação |
|---|---|---|
| Falta de tempo para manter | Média | Módulos independentes — um incompleto não invalida os outros. CONTRIBUTING.md claro para que outros contribuam. |
| Perfil nacional PT muda | Baixa | Monitoriza o repositório do IMT e o NAP. Módulo 7 tem changelogs próprios. |
| Baixa adoção inicial | Média | Normal para projetos técnicos de nicho. Um único técnico que use o curso para implementar NeTEx num operador já justifica o esforço. |
| Conflito com iniciativa IMT | Baixa | Aborda o IMT cedo — é melhor ser colaborador do que concorrente. |

### 6.5 Indicadores de sucesso

- 30+ estrelas no GitHub ao fim de 6 meses
- Pelo menos 1 contribuidor externo ao fim de 6 meses
- Pelo menos 1 operador português a referenciar o curso
- Menção no site do IMT ou NAP Portugal
- Módulo 7 citado por alguém a implementar o perfil PT

---

## 7. Estrutura do repositório

### README.md (raiz)

Deve conter:

- O que é este curso e a quem se destina
- Pré-requisitos (conhecimento de GTFS, familiaridade com XML)
- Mapa de módulos com estado (✅ completo / 🚧 em progresso / 📋 planeado)
- Como começar: link direto para o Módulo 0
- Como contribuir: link para CONTRIBUTING.md
- Licença: CC BY 4.0

### Estrutura de um módulo típico

```
modulos/02_rede_transporte/
├── README.md                    ← conteúdo principal do módulo
├── exemplos/
│   ├── 02_01_linha_simples.xml  ← exemplo mínimo comentado
│   ├── 02_02_linha_completa.xml ← exemplo completo
│   └── comparacao_gtfs/
│       ├── routes.txt           ← equivalente GTFS
│       └── stops.txt
└── exercicios/
    ├── exercicio_01.md          ← enunciado
    └── solucoes/
        └── exercicio_01.xml     ← solução comentada
```

### Glossário (recursos/glossario.md)

Tabela com 3 colunas: termo NeTEx em inglês, definição em português, equivalente GTFS (quando existe). Exemplo:

| Termo NeTEx | Definição | Equivalente GTFS |
|---|---|---|
| `Line` | Conjunto de rotas agrupadas sob uma designação comercial | `routes.txt` → `route_id` |
| `ServiceJourney` | Uma viagem específica num dia específico | `trips.txt` → `trip_id` |
| `ScheduledStopPoint` | Ponto de paragem lógico independente da infraestrutura física | `stops.txt` → `stop_id` |
| `StopPlace` | Local físico de paragem com todos os seus acessos e plataformas | `stops.txt` com `location_type=1` |
| `DayType` | Tipo de dia de serviço (ex: dia útil, sábado, feriado) | `calendar.txt` |

---

## 8. Recursos e referências

### Projeto de referência

- [github.com/etalab/netex-france-formation](https://github.com/etalab/netex-france-formation) — curso francês que este projeto adapta e expande

### NeTEx e Transmodel

- [transmodel-cen.eu](https://transmodel-cen.eu) — site oficial do consórcio
- [github.com/NeTEx-CEN/NeTEx](https://github.com/NeTEx-CEN/NeTEx) — schema XSD e exemplos XML oficiais
- [normes.transport.data.gouv.fr](https://normes.transport.data.gouv.fr) — perfil nacional francês (referência de boas práticas)

### Portugal

- [ptprofiles.azurewebsites.net](https://ptprofiles.azurewebsites.net) — perfil nacional NeTEx Portugal
- [nap-portugal.imt-ip.pt](https://nap-portugal.imt-ip.pt) — Ponto de Acesso Nacional português
- [imt-ip.pt](https://www.imt-ip.pt) — Instituto da Mobilidade e dos Transportes

### Ferramentas

- [github.com/entur/netex-validator-java](https://github.com/entur/netex-validator-java) — validador NeTEx da Entur
- [github.com/MobilityData/gtfs-validator](https://github.com/MobilityData/gtfs-validator) — para contexto de comparação GTFS
- [opentransportdata.swiss/en/cookbook/timetable-cookbook/netex](https://opentransportdata.swiss/en/cookbook/timetable-cookbook/netex/) — cookbook NeTEx em inglês (Suíça)

### Dados abertos portugueses

- [dados.gov.pt](https://dados.gov.pt) — portal de dados abertos do governo português
- [transitfeeds.com](https://transitfeeds.com) / [Mobility Database](https://mobilitydatabase.org) — feeds GTFS de operadores portugueses

### Regulação

- Regulamento Delegado (UE) 2024/490 (MMTIS) — obrigação de partilha de dados de transporte
- Diretiva ITS 2010/40/UE — base legal para standards NeTEx/SIRI na Europa

---

*Plano criado com base na análise do projeto `etalab/netex-france-formation` e no contexto de implementação NeTEx em Portugal. Versão 1.0.*
