# Exercício 6 — Ciclo de Vida de um Feed GTFS

## Contexto

A **TransBeira** é um operador fictício de transporte público que serve a região de Viseu. Opera 12 linhas urbanas e 8 linhas suburbanas. Atualmente, os horários são geridos numa folha de cálculo Excel e o operador nunca publicou um feed GTFS. O IMT notificou a TransBeira de que, ao abrigo do Regulamento (UE) 2024/490 (MMTIS), deve disponibilizar dados no NAP Portugal.

---

## Parte 1 — Auditoria de Erros de Validação

O validador Canonical GTFS Validator foi executado sobre um feed GTFS de teste e produziu os seguintes resultados. Classifique cada problema como **ERROR**, **WARNING** ou **INFO**, e justifique brevemente.

| # | Problema detetado | Nível? | Justificação |
|---|-------------------|--------|-------------|
| 1 | O `stop_id` "VISEU_P42" é referenciado em `stop_times.txt` mas não existe em `stops.txt` | | |
| 2 | A paragem "Praça da República" tem coordenadas `stop_lat=0.0`, `stop_lon=0.0` | | |
| 3 | O ficheiro `feed_info.txt` não contém `feed_start_date` nem `feed_end_date` | | |
| 4 | A velocidade implícita entre as paragens "Repeses" e "Abraveses" é de 187 km/h | | |
| 5 | O ficheiro `shapes.txt` não existe no feed | | |
| 6 | Existem dois registos em `stops.txt` com o mesmo `stop_id` "VISEU_P15" | | |
| 7 | O campo `wheelchair_accessible` está vazio em todas as viagens de `trips.txt` | | |
| 8 | O `service_id` "DU_2026" em `calendar.txt` tem `end_date` = 20250630 (data já passada) | | |

> **Dica**: recorde os três níveis do validador — ERROR (violação da especificação), WARNING (desvio das boas práticas), INFO (sugestão de melhoria).

---

## Parte 2 — Desenhar um Pipeline de Publicação

A TransBeira precisa de criar o seu primeiro pipeline de produção GTFS. Desenhe o processo completo, respondendo às seguintes questões:

### 2.1 Fontes de dados

Liste as fontes de dados que a TransBeira precisará de utilizar e o que cada uma contribui para o feed GTFS:

| Fonte | Dados contribuídos | Ficheiros GTFS afetados |
|-------|-------------------|------------------------|
| | | |
| | | |
| | | |

### 2.2 Etapas do pipeline

Ordene e descreva as etapas do pipeline, desde os dados brutos até à publicação no NAP Portugal:

1. **Recolha**: ...
2. **Transformação**: ...
3. **Validação**: ...
4. **Revisão**: ...
5. **Publicação**: ...
6. **Monitorização**: ...

Para cada etapa, indique:
- Quem é responsável (ex: equipa de planeamento, equipa de TI, direção)
- Que ferramenta ou sistema é utilizado
- Qual é o critério de passagem para a etapa seguinte

### 2.3 Frequência e calendarização

A TransBeira tem dois períodos de horários distintos: Inverno (Setembro-Junho) e Verão (Julho-Agosto). Proponha:

- Com que antecedência o novo feed deve ser publicado antes da mudança de horário
- Que procedimento seguir para correções urgentes (ex: erro de coordenadas detetado por um utilizador do Google Maps)
- Como gerir o período de sobreposição entre o feed antigo e o novo

---

## Parte 3 — Criar `feed_info.txt`

Crie o ficheiro `feed_info.txt` para a TransBeira com os seguintes dados:

- **Nome do publicador**: TransBeira — Transportes Públicos de Viseu
- **URL do publicador**: https://www.transbeira.pt
- **Idioma**: português
- **Período de validade**: 1 de Setembro de 2026 a 30 de Junho de 2027
- **Versão**: Inverno_2026_v1
- **Email de contacto**: gtfs@transbeira.pt

Escreva o ficheiro CSV completo (cabeçalho + linha de dados):

```csv
(a sua resposta aqui)
```

---

## Questões de Reflexão

1. **Qualidade vs velocidade**: a TransBeira detetou um erro numa coordenada de paragem 2 dias antes da entrada em vigor do novo horário de Inverno. O feed já está publicado no NAP. Que opções tem o operador? Qual recomendaria e porquê?

2. **Versionamento**: dois feeds consecutivos da TransBeira têm `feed_version` "v1" e "v2". Um consumidor (ex: Google Transit) reporta que a Linha 3 desapareceu na versão v2. Que informação deveria existir para diagnosticar o problema rapidamente? Como evitar esta situação no futuro?

3. **Obrigações regulatórias**: o Regulamento (UE) 2024/490 exige a disponibilização de dados de transporte nos NAP nacionais. Na prática, que consequências pode ter para um operador português a não publicação de um feed atualizado?

4. **GTFS vs NeTEx**: o NAP Portugal aceita ambos os formatos. Para um operador pequeno como a TransBeira (12+8 linhas), qual recomendaria como primeiro passo e porquê?

---

## Checklist de Auto-avaliação

- [ ] Classifiquei corretamente os 8 problemas de validação por nível de severidade
- [ ] Distingui entre violações da especificação (ERROR) e desvios de boas práticas (WARNING)
- [ ] Desenhei um pipeline completo com pelo menos 5 etapas
- [ ] Identifiquei responsáveis e ferramentas para cada etapa do pipeline
- [ ] Criei um `feed_info.txt` válido com todos os campos recomendados
- [ ] Refleti sobre os desafios reais de manutenção de um feed GTFS
- [ ] Compreendi as obrigações de publicação no contexto regulatório europeu
