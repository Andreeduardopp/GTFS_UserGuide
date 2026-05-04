# Erros Frequentes em Feeds GTFS Portugueses

Catálogo de erros e avisos comuns detetados pelo MobilityData Canonical GTFS Validator em feeds de operadores portugueses.

---

## Erros (ERROR) — Feed Inválido

### 1. `foreign_key_violation`

**Descrição**: Um ID referenciado não existe no ficheiro de destino.

**Exemplo**:
```
stop_times.txt, linha 142: stop_id="XYZ99" não existe em stops.txt
```

**Causa**: Paragem removida de `stops.txt` mas ainda referenciada em `stop_times.txt`. Comum após reorganização de paragens.

**Solução**: Garantir consistência referencial — ao remover uma paragem, atualizar todos os ficheiros que a referenciam.

---

### 2. `duplicate_key`

**Descrição**: Dois registos com o mesmo ID primário.

**Exemplo**:
```
stops.txt: stop_id="BLR1" aparece nas linhas 15 e 203
```

**Causa**: Erro na geração do feed — duas paragens diferentes com o mesmo ID, ou paragem duplicada.

**Solução**: Garantir IDs únicos. Verificar script de geração.

---

### 3. `missing_required_field`

**Descrição**: Campo obrigatório em branco.

**Exemplo**:
```
routes.txt, linha 5: route_type está vazio
```

**Causa**: Exportação incompleta do software de planeamento.

**Solução**: Preencher todos os campos obrigatórios antes de publicar.

---

## Avisos (WARNING) — Desvio das Best Practices

### 4. `fast_travel_between_stops`

**Descrição**: A velocidade implícita entre duas paragens consecutivas excede limites razoáveis.

**Exemplo**:
```
trip_id="200_DU_0600": entre stop_sequence 3 e 4, velocidade=180 km/h (limite para bus=150 km/h)
```

**Causa**: Coordenadas incorretas de uma paragem (troca de lat/lon) ou tempo de viagem demasiado curto.

**Solução**: Verificar coordenadas das paragens envolvidas e tempos em `stop_times.txt`.

---

### 5. `stop_too_far_from_shape`

**Descrição**: Uma paragem está a mais de 150 metros da shape da viagem.

**Exemplo**:
```
trip_id="200_DU_0600", stop_id="PRG4": distância à shape = 230m
```

**Causa**: Shape desatualizada após alteração de percurso, ou coordenadas da paragem imprecisas.

**Solução**: Atualizar shapes para refletir o percurso real ou corrigir coordenadas.

---

### 6. `missing_feed_info_date`

**Descrição**: `feed_info.txt` não contém `feed_start_date` ou `feed_end_date`.

**Causa**: `feed_info.txt` criado manualmente sem campos opcionais (mas recomendados).

**Solução**: Adicionar `feed_start_date` e `feed_end_date` ao `feed_info.txt`.

---

### 7. `unused_shape`

**Descrição**: Um `shape_id` definido em `shapes.txt` não é referenciado por nenhuma viagem.

**Causa**: Shape de um percurso descontinuado que permanece no ficheiro.

**Solução**: Remover shapes não utilizadas antes de publicar.

---

### 8. `decreasing_stop_time_distance`

**Descrição**: `shape_dist_traveled` não é estritamente crescente dentro de uma viagem.

**Exemplo**:
```
trip_id="200_DU_0600": stop_sequence 3 tem dist=3400m, stop_sequence 4 tem dist=3200m
```

**Causa**: Erro no cálculo de distâncias ou paragens fora de ordem.

**Solução**: Recalcular `shape_dist_traveled` usando projeção das paragens na shape.

---

## Informações (INFO)

### 9. `unknown_column`

**Descrição**: Coluna não reconhecida pela especificação GTFS.

**Exemplo**:
```
stops.txt contém coluna "stop_code_internal" — não é um campo GTFS
```

**Causa**: Exportação inclui campos internos do operador.

**Solução**: Remover colunas não-standard ou prefixar com `_` (convenção informal).

---

### 10. `empty_column_name`

**Descrição**: Cabeçalho com nome em branco.

**Causa**: Vírgula extra no final da linha de cabeçalho.

**Solução**: Remover vírgulas finais dos cabeçalhos.

---

## Checklist de Validação

Antes de publicar um feed, verificar:

- [ ] Zero ERRORs no relatório do validador
- [ ] WARNINGs revistos e corrigidos (ou justificados)
- [ ] `feed_info.txt` com datas de validade
- [ ] Todos os `service_id` em `trips.txt` existem no calendário
- [ ] Coordenadas das paragens verificadas (não trocadas, não em 0,0)
- [ ] Shapes presentes e próximas das paragens
- [ ] Feed ZIP não contém ficheiros extra (ex: `.DS_Store`, `Thumbs.db`)
