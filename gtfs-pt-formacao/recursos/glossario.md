# Glossário GTFS PT-EN

| Termo GTFS (EN) | Definição (PT) | Ficheiro GTFS |
|---|---|---|
| `agency` | Operador de transporte público que fornece o serviço | `agency.txt` |
| `route` | Linha de transporte com identidade comercial (ex: Linha 200) | `routes.txt` |
| `route_type` | Código numérico que identifica o modo de transporte (3=autocarro, 0=eléctrico, 2=comboio) | `routes.txt` |
| `trip` | Viagem específica de um veículo numa rota, num dia definido | `trips.txt` |
| `stop` | Local físico de paragem com coordenadas geográficas | `stops.txt` |
| `stop_time` | Hora de chegada e partida de uma viagem numa paragem específica | `stop_times.txt` |
| `stop_sequence` | Ordem de passagem de uma viagem pelas suas paragens (inteiro crescente) | `stop_times.txt` |
| `service_id` | Identificador de um padrão de serviço (liga viagens a calendários) | `trips.txt`, `calendar.txt` |
| `calendar` | Padrão semanal de serviço (segunda a domingo) com datas de início e fim | `calendar.txt` |
| `calendar_date` | Exceção ao calendário base (adição ou remoção de serviço numa data específica) | `calendar_dates.txt` |
| `shape` | Sequência de pontos GPS que descreve a geometria de uma rota no mapa | `shapes.txt` |
| `direction_id` | Indicador binário (0 ou 1) da direção de uma viagem numa rota | `trips.txt` |
| `block_id` | Identificador de bloco operacional (sequência de viagens do mesmo veículo) | `trips.txt` |
| `timepoint` | Indica se os tempos de passagem são exatos (1) ou aproximados (0) | `stop_times.txt` |
| `parent_station` | Referência à estação-pai que agrupa várias paragens/plataformas | `stops.txt` |
| `location_type` | Tipo de local: 0=paragem, 1=estação, 2=entrada/saída, 3=nó genérico, 4=zona de embarque | `stops.txt` |
| `wheelchair_accessible` | Indica se o veículo da viagem é acessível a cadeira de rodas (0=desconhecido, 1=sim, 2=não) | `trips.txt` |
| `wheelchair_boarding` | Indica se a paragem permite embarque em cadeira de rodas (0=desconhecido, 1=sim, 2=não) | `stops.txt` |
| `fare_attribute` | Atributo tarifário: preço, moeda, tipo de pagamento | `fare_attributes.txt` |
| `fare_rule` | Regra que liga uma tarifa a rotas, zonas de origem e zonas de destino | `fare_rules.txt` |
| `transfer` | Regra de transferência entre duas paragens (tempo mínimo, tipo de transferência) | `transfers.txt` |
| `pathway` | Percurso pedonal dentro de uma estação (corredores, escadas, elevadores) | `pathways.txt` |
| `frequency` | Serviço baseado em frequência (headway) em vez de horários exatos | `frequencies.txt` |
| `feed_info` | Metadados do feed: nome do publicador, URL, idioma, datas de validade | `feed_info.txt` |
| `headway` | Intervalo de tempo entre veículos consecutivos na mesma rota | `frequencies.txt` |

---

*Este glossário será expandido ao longo do desenvolvimento dos módulos.*
