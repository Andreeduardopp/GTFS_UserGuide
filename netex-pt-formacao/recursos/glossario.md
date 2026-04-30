# Glossário NeTEx PT-EN

| Termo NeTEx (EN) | Definição (PT) | Equivalente GTFS |
|---|---|---|
| `Line` | Conjunto de rotas agrupadas sob uma designação comercial | `routes.txt` → `route_id` |
| `Route` | Percurso específico seguido por uma linha | — |
| `RoutePoint` | Ponto de passagem numa rota | — |
| `ScheduledStopPoint` | Ponto de paragem lógico independente da infraestrutura física | `stops.txt` → `stop_id` |
| `StopPlace` | Local físico de paragem com todos os seus acessos e plataformas | `stops.txt` com `location_type=1` |
| `ServiceJourney` | Uma viagem específica num dia específico | `trips.txt` → `trip_id` |
| `TimetableFrame` | Contentor para horários e viagens de serviço | — |
| `DayType` | Tipo de dia de serviço (ex: dia útil, sábado, feriado) | `calendar.txt` |
| `DayTypeAssignment` | Associação de um `DayType` a datas específicas | `calendar_dates.txt` |
| `Quay` | Plataforma específica dentro de um `StopPlace` | `stops.txt` com `location_type=0` |
| `FareFrame` | Contentor para informação tarifária | `fare_attributes.txt` |
| `TariffZone` | Zona tarifária geográfica | `fare_rules.txt` → `origin_id` / `destination_id` |
| `FareProduct` | Produto tarifário (bilhete, passe) | — |
| `AccessibilityAssessment` | Avaliação de acessibilidade de um local ou serviço | Campos `wheelchair_*` |
| `VersionFrame` | Contentor versionado que agrupa entidades NeTEx | — |

---

*Este glossário será expandido ao longo do desenvolvimento dos módulos.*
