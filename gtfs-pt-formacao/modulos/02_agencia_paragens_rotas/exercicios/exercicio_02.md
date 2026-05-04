# Exercício 2 — Criar Agency, Stops e Routes para um Operador

## Contexto

Imagine que está a criar o feed GTFS para a **CP — Comboios de Portugal**, especificamente para a **Linha de Cascais** (Cais do Sodré → Cascais). Crie os três ficheiros base.

---

## Parte 1 — `agency.txt`

Crie o ficheiro `agency.txt` para a CP com os seguintes dados:
- ID: `CP`
- Nome: `Comboios de Portugal`
- URL: `https://www.cp.pt`
- Timezone: `Europe/Lisbon`
- Idioma: `pt`

---

## Parte 2 — `stops.txt`

Crie o ficheiro `stops.txt` com as seguintes estações da Linha de Cascais:

| Nome | Lat | Lon |
|------|-----|-----|
| Cais do Sodré | 38.7069 | -9.1441 |
| Santos | 38.7044 | -9.1548 |
| Alcântara-Mar | 38.7029 | -9.1683 |
| Belém | 38.6968 | -9.2008 |
| Algés | 38.6976 | -9.2278 |
| Oeiras | 38.6906 | -9.3120 |

Para cada estação, defina `location_type=1` (estação ferroviária). Adicione pelo menos duas plataformas (`location_type=0`) para a estação de Cais do Sodré, com `parent_station` correto.

---

## Parte 3 — `routes.txt`

Crie o ficheiro `routes.txt` para a Linha de Cascais:
- `route_type=2` (comboio)
- Escolha cores apropriadas (a Linha de Cascais usa tipicamente azul claro)

---

## Questões de Reflexão

1. Qual é a diferença entre uma estação (`location_type=1`) e uma paragem (`location_type=0`)? Quando é que faz sentido usar a hierarquia `parent_station`?
2. Se a CP e a Fertagus partilhassem a estação de Entrecampos, como modelaria isso em `stops.txt`?
3. Porque é importante que o `stop_id` de cada estação se mantenha consistente entre atualizações do feed?

---

## Checklist de Auto-avaliação

- [ ] `agency.txt` tem todos os campos obrigatórios preenchidos
- [ ] `stops.txt` tem estações com `location_type=1` e plataformas com `location_type=0`
- [ ] As plataformas referenciam a estação-pai via `parent_station`
- [ ] `routes.txt` tem `route_type=2` correto para comboio
- [ ] `agency_id` em `routes.txt` corresponde ao `agency_id` em `agency.txt`
