# Exercício 3 — Criar Viagens e Horários

## Contexto

Continuando com a **Linha de Cascais** (CP) do Exercício 2, vamos agora criar viagens e horários.

---

## Parte 1 — `trips.txt`

Crie 4 viagens para a Linha de Cascais:

1. Viagem das 07:15, dias úteis, direção Cascais
2. Viagem das 08:00, dias úteis, direção Cascais
3. Viagem das 07:30, dias úteis, direção Cais do Sodré (volta)
4. Viagem das 09:00, sábados, direção Cascais

Use `direction_id` para distinguir ida (0) e volta (1). Atribua `wheelchair_accessible=1` a todas as viagens.

---

## Parte 2 — `stop_times.txt`

Para a viagem das 07:15 (Cais do Sodré → Cascais), crie os horários de passagem usando as 6 estações do Exercício 2:

| Estação | Chegada | Partida |
|---------|---------|---------|
| Cais do Sodré | — | 07:15:00 |
| Santos | 07:18:00 | 07:18:00 |
| Alcântara-Mar | 07:22:00 | 07:22:00 |
| Belém | 07:27:00 | 07:28:00 |
| Algés | 07:32:00 | 07:32:00 |
| Oeiras | 07:45:00 | — |

Notas:
- Na primeira paragem, não há `arrival_time` (ou é igual ao `departure_time`)
- Na última paragem, não há `departure_time` (ou é igual ao `arrival_time`)
- Em Belém, o comboio para 1 minuto (chegada 07:27, partida 07:28)

---

## Parte 3 — Viagem Noturna

Crie uma viagem que parte de Cais do Sodré às 23:45 e chega a Oeiras às 00:30 do dia seguinte. Use o formato de hora GTFS correto para tempos após a meia-noite.

---

## Questões de Reflexão

1. Porque é que o GTFS usa tempos superiores a `24:00:00` em vez de voltar a `00:00:00`? Que problemas evita?
2. Se a CP tivesse um serviço expresso que não para em Santos e Alcântara-Mar, como modelaria isso em `stop_times.txt`?
3. Qual a diferença entre omitir `arrival_time`/`departure_time` e usar `timepoint=0`?

---

## Checklist de Auto-avaliação

- [ ] Criei viagens com `direction_id` correto (0=ida, 1=volta)
- [ ] Cada viagem referencia um `service_id` e `route_id` válidos
- [ ] `stop_sequence` é estritamente crescente em cada viagem
- [ ] A viagem noturna usa tempos >24:00:00 para após a meia-noite
- [ ] `arrival_time` e `departure_time` são coerentes (arrival <= departure)
