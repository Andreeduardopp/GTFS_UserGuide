# Changelog

Todas as alterações relevantes a este projeto serão documentadas neste ficheiro.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/).

---

## [Não publicado]

### Adicionado
- Estrutura inicial do repositório
- Plano de implementação dos módulos 00–08
- Exemplo GTFS de referência para ponte GTFS → NeTEx

---

## [0.1.0-alpha] — 2026-04-30

### CURSO COMPLETO — Todos os 9 Módulos Concluídos

Primeira versão alpha completa do curso NeTEx PT Formação. Todos os módulos 0–8 estão implementados com conteúdo, exemplos XML comentados e exercícios com solução.

### Conteúdo Criado — Fase 3, Módulo 8 Concluído

- `modulos/08_ferramentas_praticas/README.md` — conteúdo completo:
  - Secção 1: netex-validator (Entur) — instalação, uso, interpretação de resultados, tabela de erros frequentes
  - Secção 2: Editores XML recomendados (VS Code + Red Hat XML, Oxygen)
  - Secção 3: Conversão GTFS ↔ NeTEx — estado 2026, o que se perde em cada direção, Chouette
  - Secção 4: Navegar o schema XSD — estrutura do repositório, como encontrar elementos, minOccurs
  - Secção 5: Checklist de implementação em 9 secções (41 itens) para verificar antes de submeter ao NAP
- `modulos/08_ferramentas_praticas/exemplos/08_01_erros_frequentes.xml` — 10 erros ERR-01 a ERR-10 com padrão errado (em comentário) e padrão correcto (no XML), cobrindo ParticipantRef, IDs, version, refs, Name, PublicCode, StopPlaceType, Centroid, ToDate e DepartureTime

### Estatísticas do Curso Completo

- **9 módulos** (00–08) com README completo
- **12 exemplos XML** comentados em português
- **8 exercícios** com enunciados detalhados
- **8 soluções XML** com respostas às questões de reflexão
- **Rede âncora**: STCP Porto, Linha 200 (dados reais Abr 2026)

---

## [0.0.8] — 2026-04-30

### Conteúdo Criado — Fase 3, Módulo 7 Concluído

- `modulos/07_perfil_netex_portugal/README.md` — conteúdo completo (único recurso global sobre o perfil PT):
  - Secção 1: O que é um perfil nacional e tabela de perfis europeus
  - Secção 2: Perfil PT-EPIP — estrutura M/R/O, tabelas de campos obrigatórios por frame
  - Secção 3: Convenções de identificadores IFOPT — tabela de códigos de organização e tipos
  - Secção 4: Estrutura de ficheiros ZIP para submissão ao NAP Portugal
  - Secção 5: Comparação Perfil PT vs Perfil Francês (7 dimensões)
  - Secção 6: Processo de submissão ao NAP passo-a-passo + tabela de erros comuns
- `modulos/07_perfil_netex_portugal/exemplos/07_01_checklist_submissao.xml` — NeTEx mínimo conforme com Perfil PT: todos os frames com campos M preenchidos, checklist de conformidade nos comentários, erros comuns documentados
- `modulos/07_perfil_netex_portugal/exercicios/exercicio_07.md` — ficheiro com 7 erros reais de conformidade para identificar + 3 adições em falta + 3 questões de reflexão sobre schema vs perfil, versionamento e datas de validade
- `modulos/07_perfil_netex_portugal/exercicios/solucoes/exercicio_07_solucao.xml` — todos os 7 erros corrigidos, 3 adições incluídas, respostas às questões como comentários de cabeçalho

---

## [0.0.7] — 2026-04-30

### Conteúdo Criado — Fase 3, Módulo 6 Concluído

- `modulos/06_tarifas_introducao/README.md` — conteúdo completo:
  - Secção 1: Limitações do GTFS Fares (tabela de 6 casos não modeláveis)
  - Secção 2: FareFrame — estrutura e sub-elementos
  - Secção 3: TariffZone — zonas Andante Porto Z2/Z3/Z4, ligar SSP a zonas
  - Secção 4: FareProduct e PreassignedFareProduct — tipos e exemplo Andante
  - Secção 5: SalesOfferPackage — canais de venda
  - Secção 6: Tabela GTFS Fares ↔ NeTEx (9 linhas)
  - Secção 7: Âmbito e limitações do módulo (tabela de cobertura vs módulo futuro)
- `modulos/06_tarifas_introducao/exemplos/06_01_andante_porto.xml` — FareFrame com 3 TariffZone (Z2/Z3/Z4), 4 PreassignedFareProduct (Z2/Z3/Z4 simples + 24h), 5 SalesOfferPackage (3 por zona + 2 para 24h), ServiceFrame com 4 SSP ligados às suas zonas
- `modulos/06_tarifas_introducao/exercicios/exercicio_06.md` — 4 partes (TariffZone + FareProduct + SalesOfferPackage + SSP com zonas) + 3 questões de reflexão sobre modelos tarifários, interoperabilidade e renovação mensal
- `modulos/06_tarifas_introducao/exercicios/solucoes/exercicio_06_solucao.xml` — Navegante Municipal com 1 zona, 2 produtos (normal + jovem), 3 canais de venda, 3 SSP

---

## [0.0.6] — 2026-04-30

### Conteúdo Criado — Fase 3, Módulo 5 Concluído

- `modulos/05_acessibilidade/README.md` — conteúdo completo:
  - Secção 1: O problema da representação binária do GTFS (5 casos não modeláveis)
  - Secção 2: AccessibilityAssessment e LimitationStatusEnumeration (yes/no/partial/unknown)
  - Secção 3: Campos de AccessibilityLimitation — mobilidade física e informação
  - Secção 4: AccessibilityAssessment na ServiceJourney vs StopPlace (cadeia de acessibilidade)
  - Secção 5: EPIAP (NeTEx Parte 6), campos obrigatórios para submissão ao NAP Portugal
  - Secção 6: Exemplos XML embutidos — Cenário A (totalmente acessível) vs Cenário B (com limitações)
- `modulos/05_acessibilidade/exemplos/05_01_paragem_acessivel.xml` — SiteFrame com 2 Quay (acessível vs com limitações) + TimetableFrame com 2 ServiceJourney (piso baixo com rampa vs autocarro antigo)
- `modulos/05_acessibilidade/exercicios/exercicio_05.md` — 3 partes (converter wheelchair_boarding + wheelchair_accessible + versionamento de obras) + 3 questões de reflexão
- `modulos/05_acessibilidade/exercicios/solucoes/exercicio_05_solucao.xml` — solução completa com 5 paragens, 4 viagens, versioning MFZ1 antes/depois das obras

---

## [0.0.5] — 2026-04-30

### Conteúdo Criado — Fase 3, Módulo 4 Concluído

- `modulos/04_multimodalidade/README.md` — conteúdo completo:
  - Secção 1: O problema da mistura físico/lógico no GTFS (caso Bolhão)
  - Secção 2: SiteFrame — o frame dos lugares físicos
  - Secção 3: Hierarquia StopPlace → Quay → BoardingPosition com tabela GTFS↔NeTEx
  - Secção 4: PassengerStopAssignment — ponte lógico/físico com exemplo XML
  - Secção 5: Multimodalidade — nó Bolhão (autocarro + metro) com PathLink
  - Secção 6: StopPlaceType — 7 tipos com exemplos do Porto
  - Secção 7: IFOPT e formato de ID para Portugal
- `modulos/04_multimodalidade/exemplos/04_01_paragem_bolhao.xml` — SiteFrame com StopPlace Bolhão + 2 Quay (IDA/VOLTA) + ServiceFrame com ScheduledStopPoint + PassengerStopAssignment para a Linha 200
- `modulos/04_multimodalidade/exemplos/04_02_no_intermodal.xml` — Nó intermodal Campanhã: StopPlace CP (railStation) com Quays Via 1/2, StopPlace Metro, StopPlace STCP Bus, AccessSpace átrio, 3 PathLink pedonais com TransferDuration
- `modulos/04_multimodalidade/exercicios/exercicio_04.md` — 3 partes (SiteFrame Trindade + PathLink + ServiceFrame/PSA) + 3 questões de reflexão sobre autoridade, plataformas múltiplas e versioning
- `modulos/04_multimodalidade/exercicios/solucoes/exercicio_04_solucao.xml` — solução completa com respostas às questões como comentários de cabeçalho

---

## [0.0.4] — 2026-04-30

### Conteúdo Criado — Fase 2, Módulo 3 Concluído

- `modulos/03_horarios_calendarios/README.md` — conteúdo completo:
  - Secção 1: ServiceJourney, TimetabledPassingTime, JourneyPatternRef
  - Secção 2: DayType, OperatingDay, DayTypeAssignment, ServiceCalendarFrame
  - Secção 3: TimetableFrame e relação com os outros frames
  - Secção 4: Tabela GTFS ↔ NeTEx para horários e calendários
  - Secção 5: Dados reais STCP para partida 06:00, 3 tipos de dia
- `modulos/03_horarios_calendarios/exemplos/03_horarios.xml` — ServiceCalendarFrame (3 DayType, 7 OperatingDay, 7 DayTypeAssignment) + TimetableFrame (3 ServiceJourney com dados reais)
- `modulos/03_horarios_calendarios/exercicios/exercicio_03.md` — 3 partes + 3 questões de reflexão sobre escala e acessibilidade
- `modulos/03_horarios_calendarios/exercicios/solucoes/exercicio_03_solucao.xml` — solução com respostas às questões como comentários

---

## [0.0.3] — 2026-04-30

### Conteúdo Criado — Fase 2, Módulo 2 Concluído

- `modulos/02_rede_transporte/README.md` — conteúdo completo:
  - Diagrama ASCII das 3 camadas (Comercial / Topológica / Física)
  - Secção 1.2–1.6: Line, Direction, Route, RoutePoint/SSP, StopPointInJourneyPattern
  - Tabela GTFS ↔ NeTEx para todos os campos de routes.txt
  - Tabela das 30 paragens da Linha 200 com coords e zona tarifária
- `modulos/02_rede_transporte/exemplos/02_01_linha_simples.xml` — 5 paragens, 1 Route, 1 SJP, DestinationDisplay, extensivamente comentado
- `modulos/02_rede_transporte/exemplos/02_02_linha_completa.xml` — 30 paragens IDA + 31 VOLTA, ambas as Route e SJP
- `modulos/02_rede_transporte/exemplos/comparacao_gtfs/routes_200.txt` + `stops_200.txt` — GTFS original para comparação
- `modulos/02_rede_transporte/exercicios/exercicio_02.md` — Linha 201, 4 partes, 3 questões de reflexão
- `modulos/02_rede_transporte/exercicios/solucoes/exercicio_02_solucao.xml` — solução com respostas às questões embutidas como comentários XML

---

## [0.0.2] — 2026-04-30

### Conteúdo Criado — Fase 1 Concluída

- `recursos/analise_projeto_frances.md` — análise do projeto etalab/netex-france-formation (Task 1.1)
- `exemplos/rede_exemplo_portugal/gtfs/` — feed GTFS da STCP Linha 200 extraído (Task 1.2)
- `exemplos/rede_exemplo_portugal/README.md` — decisão âncora STCP Porto documentada (Task 1.3)
- `referencias/README.md` — adicionado DATA-03 (STCP Porto)
- `recursos/comparacao_gtfs_netex.md` — expandido com equivalências conceito a conceito e tabela GTFS ↔ NeTEx completa

### Conteúdo Criado — Fase 2, Módulo 0 Concluído

- `modulos/00_antes_de_comecar/README.md` — conteúdo completo:
  - Secção 1: O que é o NeTEx (definição, origem, importância para PT)
  - Secção 2: Comparação com GTFS (tabela + equivalências + analogia BD relacional)
  - Secção 3: Ecossistema Transmodel → NeTEx → SIRI → NAP (diagrama ASCII)
  - Secção 4: Regulação EU 2024/490 (obrigações, impacto PT)
  - Secção 5: Como usar este curso (pré-requisitos, ferramentas, rede âncora)

### Conteúdo Criado — Fase 2, Módulo 1 Concluído

- `modulos/01_conceitos_base/README.md` — conteúdo completo:
  - Secção 1: Entidades, atributos e relações (EntityInVersion, hierarquia de classes)
  - Secção 2: VersionFrame e estrutura de ficheiros (tipos de frame, PublicationDelivery)
  - Secção 3: Identificadores, referências e versionamento (formato PT, `ref`, `validBetween`)
  - Secção 4: Anatomia de um ficheiro NeTEx real
- `modulos/01_conceitos_base/exemplos/01_frame_basico.xml` — XML comentado com ResourceFrame (IMT + STCP) e ServiceFrame (Linha 200 + 5 paragens)
- `modulos/01_conceitos_base/exercicios/exercicio_01.md` — exercício em 3 partes (construir IDs, corrigir erros, criar ResourceFrame)
- `modulos/01_conceitos_base/exercicios/solucoes/exercicio_01_solucao.xml` — solução comentada
