# Exercício 7 — Auditar e Corrigir um Ficheiro NeTEx com Erros de Conformidade

> *Módulo 7 — Perfil NeTEx Portugal*

---

## Contexto

Um operador de transporte enviou ao IMT um ficheiro NeTEx para submissão ao NAP Portugal. O ficheiro foi **rejeitado** pelo validador com 7 erros de conformidade com o Perfil PT.

O seu trabalho é identificar e corrigir todos os erros.

---

## Ficheiro com Erros

```xml
<?xml version="1.0" encoding="UTF-8"?>
<PublicationDelivery xmlns="http://www.netex.org.uk/netex" version="1.0">

  <PublicationTimestamp>2026-04-30T00:00:00Z</PublicationTimestamp>
  <ParticipantRef>CARRIS</ParticipantRef>

  <dataObjects>
    <CompositeFrame id="CompositeFrame_Carris_L15" version="1">
      <validBetween>
        <FromDate>2026-04-30</FromDate>
      </validBetween>
      <frames>

        <ResourceFrame id="PT:Carris:ResourceFrame:Carris_RF:LOC" version="1">
          <organisations>
            <Operator id="PT:Carris:Operator:Carris:LOC" version="1">
              <Name>Carris</Name>
              <OrganisationType>operator</OrganisationType>
            </Operator>
          </organisations>
        </ResourceFrame>

        <ServiceFrame id="PT:Carris:ServiceFrame:L15_SvF:LOC" version="1">
          <lines>
            <Line id="PT:Carris:Line:15E:LOC" version="1">
              <Name>Linha 15E — Praça da Figueira / Algés</Name>
              <TransportMode>tram</TransportMode>
              <OperatorRef ref="PT:Carris:Operator:Carris:LOC" version="1"/>
            </Line>
          </lines>
          <scheduledStopPoints>
            <ScheduledStopPoint id="PT:Carris:ScheduledStopPoint:FIG:LOC" version="0">
              <Name>Praça da Figueira</Name>
            </ScheduledStopPoint>
            <ScheduledStopPoint id="PT:Carris:ScheduledStopPoint:ALG:LOC" version="1">
              <Name>Algés</Name>
            </ScheduledStopPoint>
          </scheduledStopPoints>
          <stopAssignments>
            <PassengerStopAssignment id="PT:Carris:PSA:FIG_Figueira:LOC" version="1" order="1">
              <ScheduledStopPointRef ref="PT:Carris:ScheduledStopPoint:FIG:LOC" version="1"/>
              <StopPlaceRef ref="PT:Carris:StopPlace:Figueira:LOC" version="1"/>
              <QuayRef ref="PT:Carris:Quay:Figueira_IDA:LOC" version="1"/>
            </PassengerStopAssignment>
          </stopAssignments>
        </ServiceFrame>

        <SiteFrame id="PT:Carris:SiteFrame:L15_SF:LOC" version="1">
          <stopPlaces>
            <StopPlace id="PT:Carris:StopPlace:Figueira:LOC" version="1">
              <Name>Praça da Figueira</Name>
              <Centroid>
                <Location>
                  <Longitude>-9.137060</Longitude>
                  <Latitude>38.714870</Latitude>
                </Location>
              </Centroid>
              <quays>
                <Quay id="PT:Carris:Quay:Figueira_IDA:LOC" version="1">
                  <Name>Praça da Figueira — IDA</Name>
                  <Centroid>
                    <Location>
                      <Longitude>-9.137060</Longitude>
                      <Latitude>38.714870</Latitude>
                    </Location>
                  </Centroid>
                </Quay>
              </quays>
            </StopPlace>
          </stopPlaces>
        </SiteFrame>

      </frames>
    </CompositeFrame>
  </dataObjects>
</PublicationDelivery>
```

---

## Parte A — Identificar os Erros

O validador do NAP Portugal reportou os seguintes **7 erros**. Para cada um, explique:
- O que está errado
- Qual a regra do Perfil PT que é violada
- Como corrigir

| # | Mensagem do validador | Elemento | Linha aprox. |
|---|-----------------------|----------|-------------|
| E1 | "ParticipantRef must follow format PT:<org>" | `ParticipantRef` | 5 |
| E2 | "CompositeFrame id missing namespace prefix PT:" | `CompositeFrame/id` | 8 |
| E3 | "validBetween must have ToDate" | `validBetween` | 9-11 |
| E4 | "Operator missing mandatory field ContactDetails/Url" | `Operator` | 16-20 |
| E5 | "Line missing mandatory field PublicCode" | `Line` | 26-31 |
| E6 | "ScheduledStopPoint version must be positive integer (≥1)" | `ScheduledStopPoint:FIG` | 36 |
| E7 | "StopPlace missing mandatory field StopPlaceType" | `StopPlace:Figueira` | 58 |

---

## Parte B — Corrigir o Ficheiro

Reescreva o ficheiro com todos os 7 erros corrigidos. Acrescente também:

1. O campo `Authority` para o IMT (em falta no ResourceFrame)
2. A `Quay` da paragem Algés (em falta no SiteFrame)
3. O `PassengerStopAssignment` para a paragem Algés (em falta no ServiceFrame)

---

## Questões de Reflexão

**1. Perfil PT vs NeTEx genérico**
O ficheiro original é um XML NeTEx tecnicamente válido (sem erros de schema XSD). O validador do NAP rejeitou-o por erros de *perfil*, não de *schema*.

Explique a diferença entre validação de schema e validação de perfil. Por que é que o NAP Portugal precisa de ambas?

**2. version="0" é um erro grave?**
O erro E6 é que o `ScheduledStopPoint:FIG` tem `version="0"`. Na especificação NeTEx base, o valor `version="0"` não é explicitamente proibido.

Por que razão é que o Perfil PT exige `version` como inteiro positivo ≥1? Que problemas práticos causaria ter `version="0"` num sistema de gestão de dados?

**3. O ToDate é obrigatório?**
O erro E3 é que `validBetween` não tem `ToDate`. O perfil PT exige que os dados tenham uma data de fim de validade.

Que vantagens tem para o NAP ter datas de fim explícitas nos dados? E que desvantagens poderia ter para um operador cujos dados "nunca expiram"?

---

## Solução

Consulte [`solucoes/exercicio_07_solucao.xml`](solucoes/exercicio_07_solucao.xml)
