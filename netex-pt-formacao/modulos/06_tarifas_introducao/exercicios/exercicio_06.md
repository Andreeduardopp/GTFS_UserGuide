# Exercício 6 — Modelar o Navegante Municipal Lisboa

> **Módulo 6 — Tarifas (Introdução)**

---

## Contexto

O **Navegante** é o sistema de transportes intermodal de Lisboa, gerido pela Carris Metropolitana e operadores associados. Ao contrário do Andante (Porto), que usa zonas concêntricas, o Navegante Municipal de Lisboa é um **passe mensal por operador** para toda a área de Lisboa.

### Produtos a modelar

| Produto | Descrição | Preço indicativo |
|---------|-----------|-----------------|
| Navegante Municipal | Passe mensal — toda a rede Carris (Lisboa) | 30,00 € |
| Navegante Metropolitano | Passe mensal — Carris + CP Lisboa + Metro + Transtejo | 40,00 € |
| Navegante Jovem | Desconto 50% sobre Navegante Municipal (até 23 anos) | 15,00 € |

*Nota: verificar preços atuais em navegante.transporteslisboa.pt*

### Zonas e cobertura

O Navegante Municipal cobre **toda a cidade de Lisboa** (sem subdivisão por zonas). O Navegante Metropolitano acrescenta a Área Metropolitana de Lisboa.

Para este exercício, modelamos apenas o Navegante Municipal (1 zona, cidade de Lisboa).

---

## Parte A — FareFrame: TariffZone

Crie um `FareFrame` com uma única `TariffZone`:

- Id: `PT:Carris:TariffZone:Lisboa_Municipal:LOC`
- Nome: "Navegante Municipal — Lisboa"

---

## Parte B — FareProduct: Passe Mensal

Crie um `PreassignedFareProduct` para o Navegante Municipal:

- Id: `PT:Carris:FareProduct:Navegante_Municipal:LOC`
- Nome: "Navegante Municipal"
- `TravelDocumentType=electronicDocument` (cartão Navegante)
- `RequiresCard=true`

Adicione um segundo produto para o desconto Jovem:

- Id: `PT:Carris:FareProduct:Navegante_Jovem:LOC`
- Nome: "Navegante Municipal — Jovem (até 23 anos)"
- Mesmas condições, mas nota de que é um `SaleDiscountRight` sobre o produto base

*Dica: para o Navegante Jovem, pode usar o mesmo tipo `PreassignedFareProduct` com um nome descritivo — a modelação completa de descontos usa `SaleDiscountRight`, que está fora do âmbito introdutório.*

---

## Parte C — SalesOfferPackage: Canais de Venda

O Navegante pode ser adquirido em:
1. Máquinas automáticas (estações de metro, interfaces)
2. App "Lisboa Move-Me" (canal digital)
3. Balcões Carris

Crie três `SalesOfferPackage` para o Navegante Municipal, um por canal de venda.

---

## Parte D — ServiceFrame: Ligar Paragens à Zona

Considere as seguintes paragens hipotéticas de uma linha Carris em Lisboa:

| stop_id | stop_name | zone_id (GTFS) |
|---------|-----------|----------------|
| ROS1 | Rossio | Lisboa |
| MRQ1 | Marquês de Pombal | Lisboa |
| BEL1 | Belém | Lisboa |

Crie três `ScheduledStopPoint` com `TariffZoneRef` apontando para a `TariffZone` definida na Parte A.

---

## Questões de Reflexão

**1. Andante (zonas concêntricas) vs Navegante (passe plano)**
O sistema Andante de Porto usa zonas Z2–Z10 (preço depende da distância). O Navegante Municipal de Lisboa é um preço fixo para toda a cidade.

Como é que a estrutura do NeTEx — com `TariffZone` separadas e `PreassignedFareProduct` por zona — se adapta melhor ao modelo Andante do que ao Navegante? O que teria de mudar no modelo NeTEx para um sistema de passe plano?

**2. Interoperabilidade**
Um passageiro com Navegante Metropolitano pode usar o passe em Carris, Metro de Lisboa, CP (linhas de Lisboa) e Transtejo (ferries). Em GTFS, cada operador tem o seu feed e não há forma de modelar que um passe é válido em múltiplos operadores.

Como descreveria no NeTEx que um `PreassignedFareProduct` é válido para múltiplos operadores? (Dica: pense em `Authority` vs `Operator` e em como o `FareFrame` pode referenciar vários operadores.)

**3. Renovação mensal**
Um passe mensal Navegante é válido por 1 mês a partir da data de ativação e pode ser renovado automaticamente (débito direto). Em GTFS este conceito não existe.

Como usaria o elemento `UsageValidityPeriod` do NeTEx para modelar: (a) a validade de 1 mês, (b) a renovação automática? Que valores de `UsageTrigger` faria sentido usar?

---

## Autoavaliação

- [ ] A `TariffZone` tem `id` no formato `PT:<operador>:TariffZone:<local>:LOC`?
- [ ] Os `ScheduledStopPoint` têm `TariffZoneRef` apontando para a zona correta?
- [ ] O `PreassignedFareProduct` tem `conditionSummary` com `TravelDocumentType` e `RequiresCard`?
- [ ] Existem pelo menos 3 `SalesOfferPackage` (um por canal de venda)?
- [ ] Cada `SalesOfferPackageElement` referencia o `FareProductRef` correto?
- [ ] O Navegante Jovem está modelado como produto separado com nota sobre `SaleDiscountRight`?

---

## Solução

Consulte [`solucoes/exercicio_06_solucao.xml`](solucoes/exercicio_06_solucao.xml)
