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
