- [Exemplos de Queries PromQL - Day 3](#exemplos-de-queries-promql---day-3)
  - [Exporters Disponíveis](#exporters-disponíveis)
  - [1. Queries com a Função `rate`](#1-queries-com-a-função-rate)
    - [1.1 Taxa de requisições HTTP no Prometheus](#11-taxa-de-requisições-http-no-prometheus)
    - [1.2 Taxa de requisições por handler](#12-taxa-de-requisições-por-handler)
    - [1.3 Taxa de consumo de CPU do Primeiro Exporter](#13-taxa-de-consumo-de-cpu-do-primeiro-exporter)
  - [2. Queries com a Função `irate`](#2-queries-com-a-função-irate)
    - [2.1 Taxa instantânea de requisições](#21-taxa-instantânea-de-requisições)
    - [2.2 Taxa instantânea de CPU do Segundo Exporter](#22-taxa-instantânea-de-cpu-do-segundo-exporter)
  - [3. Queries com a Função `increase`](#3-queries-com-a-função-increase)
    - [3.1 Aumento total de requisições](#31-aumento-total-de-requisições)
    - [3.2 Aumento de erros HTTP](#32-aumento-de-erros-http)
  - [4. Queries com a Função `sum`](#4-queries-com-a-função-sum)
    - [4.1 Total de memória alocada por todos os exporters](#41-total-de-memória-alocada-por-todos-os-exporters)
    - [4.2 Total de requisições por código de status](#42-total-de-requisições-por-código-de-status)
    - [4.3 Taxa total de requisições de todos os exporters](#43-taxa-total-de-requisições-de-todos-os-exporters)
  - [5. Queries com a Função `avg`](#5-queries-com-a-função-avg)
    - [5.1 Média de CPU por instância](#51-média-de-cpu-por-instância)
    - [5.2 Média de memória livre (Segundo Exporter)](#52-média-de-memória-livre-segundo-exporter)
  - [6. Queries com a Função `max` e `min`](#6-queries-com-a-função-max-e-min)
    - [6.1 Maior consumo de memória](#61-maior-consumo-de-memória)
    - [6.2 Menor memória livre](#62-menor-memória-livre)
    - [6.3 Pico de requisições](#63-pico-de-requisições)
  - [7. Queries com Funções `*_over_time`](#7-queries-com-funções-_over_time)
    - [7.1 Média de memória livre ao longo do tempo](#71-média-de-memória-livre-ao-longo-do-tempo)
    - [7.2 Soma de requisições ao longo do tempo](#72-soma-de-requisições-ao-longo-do-tempo)
    - [7.3 Máximo de CPU durante um período](#73-máximo-de-cpu-durante-um-período)
    - [7.4 Mínimo de memória livre durante um período](#74-mínimo-de-memória-livre-durante-um-período)
    - [7.5 Desvio padrão de requisições](#75-desvio-padrão-de-requisições)
  - [8. Queries com a Função `count`](#8-queries-com-a-função-count)
    - [8.1 Quantidade de métricas por job](#81-quantidade-de-métricas-por-job)
    - [8.2 Quantidade de instâncias ativas](#82-quantidade-de-instâncias-ativas)
  - [9. Queries com Agrupamento (`by` e `without`)](#9-queries-com-agrupamento-by-e-without)
    - [9.1 Requisições totais por job](#91-requisições-totais-por-job)
    - [9.2 Requisições sem o label handler](#92-requisições-sem-o-label-handler)
    - [9.3 CPU por job e instância](#93-cpu-por-job-e-instância)
  - [10. Queries com Operadores](#10-queries-com-operadores)
    - [10.1 Memória livre abaixo de 1GB](#101-memória-livre-abaixo-de-1gb)
    - [10.2 Targets inativos](#102-targets-inativos)
    - [10.3 Requisições com erro](#103-requisições-com-erro)
    - [10.4 Alta taxa de requisições](#104-alta-taxa-de-requisições)
  - [11. Queries Combinadas e Avançadas](#11-queries-combinadas-e-avançadas)
    - [11.1 Percentual de memória livre](#111-percentual-de-memória-livre)
    - [11.2 Taxa de erro (4xx e 5xx)](#112-taxa-de-erro-4xx-e-5xx)
    - [11.3 Comparação de CPU entre exporters](#113-comparação-de-cpu-entre-exporters)
    - [11.4 Crescimento de requisições comparado](#114-crescimento-de-requisições-comparado)
    - [11.5 Memória total do sistema em GB](#115-memória-total-do-sistema-em-gb)
    - [11.6 Disponibilidade média dos targets](#116-disponibilidade-média-dos-targets)
  - [12. Queries para Node Exporter](#12-queries-para-node-exporter)
    - [12.1 Uso de CPU do sistema](#121-uso-de-cpu-do-sistema)
    - [12.2 Memória disponível do sistema](#122-memória-disponível-do-sistema)
    - [12.3 Espaço em disco disponível](#123-espaço-em-disco-disponível)
    - [12.4 Taxa de I/O de disco](#124-taxa-de-io-de-disco)
    - [12.5 Network traffic](#125-network-traffic)
  - [13. Queries para Debugging e Troubleshooting](#13-queries-para-debugging-e-troubleshooting)
    - [13.1 Verificar se todos os targets estão up](#131-verificar-se-todos-os-targets-estão-up)
    - [13.2 Tempo desde o último scrape bem-sucedido](#132-tempo-desde-o-último-scrape-bem-sucedido)
    - [13.3 Duração dos scrapes](#133-duração-dos-scrapes)
    - [13.4 Tamanho das amostras coletadas](#134-tamanho-das-amostras-coletadas)
  - [14. Queries de Capacidade e Planejamento](#14-queries-de-capacidade-e-planejamento)
    - [14.1 Previsão de memória livre (tendência linear)](#141-previsão-de-memória-livre-tendência-linear)
    - [14.2 Crescimento do uso de disco](#142-crescimento-do-uso-de-disco)
  - [Dicas de Uso](#dicas-de-uso)
  - [Referências](#referências)


# Exemplos de Queries PromQL - Day 3

Este documento contém exemplos práticos de queries PromQL usando os exporters desenvolvidos até agora, organizados por função e tipo de informação extraída.

## Exporters Disponíveis

- **Prometheus** (localhost:9090) - Auto monitoramento
- **Meu Primeiro Exporter** (localhost:8899) - Exporter em Python
- **Segundo Exporter** (localhost:7788) - Exporter em Go com métricas de memória
- **Node Exporter** (localhost:9100) - Métricas do sistema operacional

---

## 1. Queries com a Função `rate`

### 1.1 Taxa de requisições HTTP no Prometheus

```promql
rate(prometheus_http_requests_total[5m])
```

**O que retorna:** Taxa média de crescimento por segundo das requisições HTTP nos últimos 5 minutos.

**Informação extraída:** Quantas requisições por segundo o Prometheus está recebendo em média.

### 1.2 Taxa de requisições por handler

```promql
rate(prometheus_http_requests_total{handler="/metrics"}[5m])
```

**O que retorna:** Taxa de requisições especificamente no endpoint `/metrics`.

**Informação extraída:** Com que frequência as métricas estão sendo consultadas.

### 1.3 Taxa de consumo de CPU do Primeiro Exporter

```promql
rate(process_cpu_seconds_total{job="Meu Primeiro Exporter"}[1m])
```

**O que retorna:** Taxa média de uso de CPU por segundo no último minuto.

**Informação extraída:** Percentual de uso de CPU do exporter (valores entre 0 e número de cores).

---

## 2. Queries com a Função `irate`

### 2.1 Taxa instantânea de requisições

```promql
irate(prometheus_http_requests_total[5m])
```

**O que retorna:** Taxa de crescimento instantânea baseada nos dois últimos pontos.

**Informação extraída:** Picos e quedas instantâneas de requisições (mais sensível que `rate`).

### 2.2 Taxa instantânea de CPU do Segundo Exporter

```promql
irate(process_cpu_seconds_total{job="segundo-exporter"}[2m])
```

**O que retorna:** Taxa instantânea de uso de CPU.

**Informação extraída:** Variações bruscas no consumo de CPU.

---

## 3. Queries com a Função `increase`

### 3.1 Aumento total de requisições

```promql
increase(prometheus_http_requests_total{handler="/api/v1/query"}[10m])
```

**O que retorna:** Total de requisições incrementadas nos últimos 10 minutos.

**Informação extraída:** Quantas queries foram executadas no período.

### 3.2 Aumento de erros HTTP

```promql
increase(prometheus_http_requests_total{code="500"}[1h])
```

**O que retorna:** Número de erros 500 na última hora.

**Informação extraída:** Quantos erros internos ocorreram.

---

## 4. Queries com a Função `sum`

### 4.1 Total de memória alocada por todos os exporters

```promql
sum(go_memstats_alloc_bytes)
```

**O que retorna:** Soma de memória alocada em bytes de todos os jobs Go.

**Informação extraída:** Consumo total de memória dos exporters em Go.

### 4.2 Total de requisições por código de status

```promql
sum(prometheus_http_requests_total) by (code)
```

**O que retorna:** Total de requisições agrupadas por código HTTP.

**Informação extraída:** Distribuição de respostas HTTP (200, 400, 500, etc).

### 4.3 Taxa total de requisições de todos os exporters

```promql
sum(rate(prometheus_http_requests_total[5m]))
```

**O que retorna:** Taxa combinada de requisições de todos os jobs.

**Informação extraída:** Throughput total do sistema.

---

## 5. Queries com a Função `avg`

### 5.1 Média de CPU por instância

```promql
avg(rate(process_cpu_seconds_total[1m])) by (instance)
```

**O que retorna:** Média de uso de CPU por instância.

**Informação extraída:** Qual instância está consumindo mais CPU em média.

### 5.2 Média de memória livre (Segundo Exporter)

```promql
avg(memoria_livre_megabytes)
```

**O que retorna:** Média de memória livre em MB.

**Informação extraída:** Quanto de memória está disponível em média.

---

## 6. Queries com a Função `max` e `min`

### 6.1 Maior consumo de memória

```promql
max(go_memstats_alloc_bytes) by (job)
```

**O que retorna:** Maior valor de memória alocada por job.

**Informação extraída:** Qual job está usando mais memória.

### 6.2 Menor memória livre

```promql
min(memoria_livre_megabytes)
```

**O que retorna:** Menor valor de memória livre registrado.

**Informação extraída:** Momento de menor disponibilidade de memória.

### 6.3 Pico de requisições

```promql
max(rate(prometheus_http_requests_total[5m])) by (handler)
```

**O que retorna:** Maior taxa de requisições por handler.

**Informação extraída:** Qual endpoint está mais sobrecarregado.

---

## 7. Queries com Funções `*_over_time`

### 7.1 Média de memória livre ao longo do tempo

```promql
avg_over_time(memoria_livre_megabytes[30m])
```

**O que retorna:** Média de memória livre nos últimos 30 minutos.

**Informação extraída:** Tendência de consumo de memória.

### 7.2 Soma de requisições ao longo do tempo

```promql
sum_over_time(prometheus_http_requests_total{handler="/metrics"}[1h])
```

**O que retorna:** Soma de todos os valores da métrica na última hora.

**Informação extraída:** Volume total de coletas de métricas.

### 7.3 Máximo de CPU durante um período

```promql
max_over_time(rate(process_cpu_seconds_total[1m])[10m:1m])
```

**O que retorna:** Pico de uso de CPU nos últimos 10 minutos.

**Informação extraída:** Momento de maior estresse da CPU.

### 7.4 Mínimo de memória livre durante um período

```promql
min_over_time(memoria_livre_megabytes[1h])
```

**O que retorna:** Menor valor de memória livre na última hora.

**Informação extraída:** Quanto de memória mínima estava disponível.

### 7.5 Desvio padrão de requisições

```promql
stddev_over_time(rate(prometheus_http_requests_total[1m])[10m:1m])
```

**O que retorna:** Desvio padrão da taxa de requisições.

**Informação extraída:** Volatilidade/estabilidade do tráfego (valores altos = tráfego instável).

---

## 8. Queries com a Função `count`

### 8.1 Quantidade de métricas por job

```promql
count(prometheus_http_requests_total) by (job)
```

**O que retorna:** Número de séries temporais por job.

**Informação extraída:** Cardinalidade das métricas.

### 8.2 Quantidade de instâncias ativas

```promql
count(up == 1)
```

**O que retorna:** Número de targets ativos.

**Informação extraída:** Quantos exporters estão funcionando.

---

## 9. Queries com Agrupamento (`by` e `without`)

### 9.1 Requisições totais por job

```promql
sum(prometheus_http_requests_total) by (job)
```

**O que retorna:** Total de requisições agrupadas por job.

**Informação extraída:** Distribuição de carga entre jobs.

### 9.2 Requisições sem o label handler

```promql
sum(prometheus_http_requests_total) without (handler)
```

**O que retorna:** Requisições totais removendo a granularidade de handler.

**Informação extraída:** Visão geral por job e código, sem detalhes de endpoint.

### 9.3 CPU por job e instância

```promql
sum(rate(process_cpu_seconds_total[1m])) by (job, instance)
```

**O que retorna:** Uso de CPU agrupado por job e instância.

**Informação extraída:** Qual combinação job/instância consome mais CPU.

---

## 10. Queries com Operadores

### 10.1 Memória livre abaixo de 1GB

```promql
memoria_livre_megabytes < 1024
```

**O que retorna:** Instâncias com menos de 1GB de memória livre.

**Informação extraída:** Alertas de memória baixa.

### 10.2 Targets inativos

```promql
up == 0
```

**O que retorna:** Targets que estão fora do ar.

**Informação extraída:** Monitoramento de disponibilidade.

### 10.3 Requisições com erro

```promql
prometheus_http_requests_total{code!="200"}
```

**O que retorna:** Todas as requisições que não retornaram 200.

**Informação extraída:** Volume de erros e códigos não-sucesso.

### 10.4 Alta taxa de requisições

```promql
rate(prometheus_http_requests_total[5m]) > 10
```

**O que retorna:** Endpoints com mais de 10 requisições/segundo.

**Informação extraída:** Identificar endpoints com alto tráfego.

---

## 11. Queries Combinadas e Avançadas

### 11.1 Percentual de memória livre

```promql
(memoria_livre_bytes / total_memoria_bytes) * 100
```

**O que retorna:** Percentual de memória livre em relação ao total.

**Informação extraída:** Saúde do sistema em porcentagem.

### 11.2 Taxa de erro (4xx e 5xx)

```promql
sum(rate(prometheus_http_requests_total{code=~"4..|5.."}[5m])) / sum(rate(prometheus_http_requests_total[5m])) * 100
```

**O que retorna:** Percentual de requisições com erro.

**Informação extraída:** Taxa de erro do sistema em %.

### 11.3 Comparação de CPU entre exporters

```promql
sum(rate(process_cpu_seconds_total[1m])) by (job) > 0.1
```

**O que retorna:** Jobs usando mais de 10% de CPU.

**Informação extraída:** Identificar processos pesados.

### 11.4 Crescimento de requisições comparado

```promql
rate(prometheus_http_requests_total{job="prometheus"}[5m]) / rate(prometheus_http_requests_total{job="prometheus"}[5m] offset 10m)
```

**O que retorna:** Fator de crescimento em relação a 10 minutos atrás.

**Informação extraída:** Se o tráfego está aumentando ou diminuindo.

### 11.5 Memória total do sistema em GB

```promql
total_memoria_gigabytes
```

**O que retorna:** Capacidade total de memória do sistema.

**Informação extraída:** Especificação de hardware.

### 11.6 Disponibilidade média dos targets

```promql
avg(up) * 100
```

**O que retorna:** Percentual médio de disponibilidade.

**Informação extraída:** SLA geral do monitoramento.

---

## 12. Queries para Node Exporter

### 12.1 Uso de CPU do sistema

```promql
100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

**O que retorna:** Percentual de uso de CPU do sistema.

**Informação extraída:** Quanto de CPU está sendo utilizada.

### 12.2 Memória disponível do sistema

```promql
node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes * 100
```

**O que retorna:** Percentual de memória disponível.

**Informação extraída:** Saúde da memória do servidor.

### 12.3 Espaço em disco disponível

```promql
(node_filesystem_avail_bytes{mountpoint="/"} / node_filesystem_size_bytes{mountpoint="/"}) * 100
```

**O que retorna:** Percentual de espaço livre no disco raiz.

**Informação extraída:** Risco de disco cheio.

### 12.4 Taxa de I/O de disco

```promql
rate(node_disk_read_bytes_total[5m]) + rate(node_disk_written_bytes_total[5m])
```

**O que retorna:** Taxa combinada de leitura e escrita em bytes/segundo.

**Informação extraída:** Carga de I/O no disco.

### 12.5 Network traffic

```promql
rate(node_network_receive_bytes_total{device="eth0"}[5m]) + rate(node_network_transmit_bytes_total{device="eth0"}[5m])
```

**O que retorna:** Taxa de tráfego de rede total.

**Informação extraída:** Utilização da interface de rede.

---

## 13. Queries para Debugging e Troubleshooting

### 13.1 Verificar se todos os targets estão up

```promql
up
```

**O que retorna:** Status de cada target (1 = up, 0 = down).

**Informação extraída:** Health check de todos os exporters.

### 13.2 Tempo desde o último scrape bem-sucedido

```promql
time() - timestamp(up)
```

**O que retorna:** Segundos desde a última coleta.

**Informação extraída:** Detectar scrapes atrasados.

### 13.3 Duração dos scrapes

```promql
scrape_duration_seconds
```

**O que retorna:** Quanto tempo o Prometheus leva para coletar métricas.

**Informação extraída:** Performance da coleta.

### 13.4 Tamanho das amostras coletadas

```promql
scrape_samples_scraped
```

**O que retorna:** Número de métricas coletadas por scrape.

**Informação extraída:** Volume de dados por target.

---

## 14. Queries de Capacidade e Planejamento

### 14.1 Previsão de memória livre (tendência linear)

```promql
predict_linear(memoria_livre_megabytes[1h], 3600)
```

**O que retorna:** Estimativa de memória livre daqui a 1 hora.

**Informação extraída:** Quando a memória pode acabar.

### 14.2 Crescimento do uso de disco

```promql
predict_linear(node_filesystem_avail_bytes{mountpoint="/"}[1h], 3600 * 24)
```

**O que retorna:** Estimativa de espaço em disco daqui a 24 horas.

**Informação extraída:** Planejamento de capacidade.

---

## Dicas de Uso

1. **Use intervalos adequados**: Para métricas rápidas use [1m], para análises de tendência use [1h] ou mais.

2. **Combine funções**: `sum(rate(...))` é muito comum para agregar taxas.

3. **Use `by` e `without`**: Para agrupar ou remover labels e ter diferentes níveis de granularidade.

4. **Teste no Graph**: Sempre visualize as queries na aba Graph para entender melhor o comportamento.

5. **Operadores são poderosos**: Use comparações para criar alertas e filtros.

6. **Funções `*_over_time`**: Excelentes para análise histórica e detecção de anomalias.

---

## Referências

- [PromQL Official Documentation](https://prometheus.io/docs/prometheus/latest/querying/basics/)
- [PromQL Functions](https://prometheus.io/docs/prometheus/latest/querying/functions/)
- [PromQL Operators](https://prometheus.io/docs/prometheus/latest/querying/operators/)
