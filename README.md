# Домашнее задание к занятию 14 «Средство визуализации Grafana» Малявко Сергей

## Обязательные задания

### Задание 1

1. Используя директорию [help](./help) внутри этого домашнего задания, запустите связку prometheus-grafana.
1. Зайдите в веб-интерфейс grafana, используя авторизационные данные, указанные в манифесте docker-compose.
1. Подключите поднятый вами prometheus, как источник данных.
1. Решение домашнего задания — скриншот веб-интерфейса grafana со списком подключенных Datasource.

**Ответ.**
- [Datasource](images/1.1.jpg)
- [Demo Dashboard](images/1.2.jpg)

## Задание 2

Изучите самостоятельно ресурсы:

1. [PromQL tutorial for beginners and humans](https://valyala.medium.com/promql-tutorial-for-beginners-9ab455142085).
1. [Understanding Machine CPU usage](https://www.robustperception.io/understanding-machine-cpu-usage).
1. [Introduction to PromQL, the Prometheus query language](https://grafana.com/blog/2020/02/04/introduction-to-promql-the-prometheus-query-language/).

Создайте Dashboard и в ней создайте Panels:

- утилизация CPU для nodeexporter (в процентах, 100-idle);
- CPULA 1/5/15;
- количество свободной оперативной памяти;
- количество места на файловой системе.

Для решения этого задания приведите promql-запросы для выдачи этих метрик, а также скриншот получившейся Dashboard.

**Ответ.**

- утилизация CPU для nodeexporter (в процентах, 100-idle);
```
(sum by(instance) (irate(node_cpu_seconds_total{instance="$node",job="$job", mode!="idle"}[$__rate_interval])) / on(instance) group_left sum by (instance)((irate(node_cpu_seconds_total{instance="$node",job="$job"}[$__rate_interval])))) * 100
```
- CPULA 1/5/15;
```
node_load1{job="$job",instance="$node"}
node_load5{job="$job",instance="$node"}
node_load15{job="$job",instance="$node"}
```
- количество свободной оперативной памяти;
```
100 - ((avg_over_time(node_memory_MemAvailable_bytes{instance="$node",job="$job"}[$__rate_interval]) * 100) / avg_over_time(node_memory_MemTotal_bytes{instance="$node",job="$job"}[$__rate_interval]))
```
- количество места на файловой системе.
```
100 - ((avg_over_time(node_filesystem_avail_bytes{instance="$node",job="$job",mountpoint="/",fstype!="rootfs"}[$__rate_interval]) * 100) / avg_over_time(node_filesystem_size_bytes{instance="$node",job="$job",mountpoint="/",fstype!="rootfs"}[$__rate_interval]))
```

- [Dashboard test](images/2.jpg)

## Задание 3

1. Создайте для каждой Dashboard подходящее правило alert — можно обратиться к первой лекции в блоке «Мониторинг».
2. В качестве решения задания приведите скриншот вашей итоговой Dashboard.

**Ответ.**
- [Dashboard](images/3.1.jpg)
- [Rules list](images/3.2.jpg)

## Задание 4

1. Сохраните ваш Dashboard.Для этого перейдите в настройки Dashboard, выберите в боковом меню «JSON MODEL». Далее скопируйте отображаемое json-содержимое в отдельный файл и сохраните его.
1. В качестве решения задания приведите листинг этого файла.

**Ответ.**
- [Contact points](dashboard-test.json)
