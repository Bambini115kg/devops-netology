## Домашнее задание к занятию "5.5. Оркестрация кластером Docker контейнеров на примере Docker Swarm"
### Задача 1
Дайте письменные ответы на следующие вопросы:

```
- В чём отличие режимов работы сервисов в Docker Swarm кластере: replication и global?
    В случае "replication", можно запустить несколько реплик одной задачи ,а в случае "global",
    запускается одна задача (антивирус например).
- Какой алгоритм выбора лидера используется в Docker Swarm кластере?
    RAFT. Сначала все узлы folower и все стремятся стать кандидатами с помощью таймера (рандомное время от 150 до 300мс).
    У кого таймер отработал первый, тот отправляет запросы остальным узлам, чтобы получить голоса.
    После получения болшинства голосов, становится менеджером. В случае, если таймеры отсичтывают одинаково у двух и более
    фоловеров, то процесс перезапускается. Это, если в кратце)
- Что такое Overlay Network?
    Сеть налодженная поверх другой сети (типа VPN)
```
### Задача 2
ПСоздать ваш первый Docker Swarm кластер в Яндекс.Облаке

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:

docker node ls

```
[root@node01 ~]# docker node ls
ID                            HOSTNAME             STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
n78t99qd7fkgsttv15wh99h5i *   node01.netology.yc   Ready     Active         Leader           20.10.12
wc4p9vd02ujdrlt1am8jrhrbb     node02.netology.yc   Ready     Active         Reachable        20.10.12
uda4kib2z48rft7n7gyzhu8ih     node03.netology.yc   Ready     Active         Reachable        20.10.12
7nv29ygsyoq46l8tz0g69dlfk     node04.netology.yc   Ready     Active                          20.10.12
iso1ev4uycsp408wa4h63wrjs     node05.netology.yc   Down      Active                          20.10.12
nwhjpd9e7zyqe2t2xd19mxvju     node06.netology.yc   Ready     Active                          20.10.12
[root@node01 ~]#
```
### Задача 3
Создать ваш первый, готовый к боевой эксплуатации кластер мониторинга, состоящий из стека микросервисов.

Для получения зачета, вам необходимо предоставить скриншот из терминала (консоли), с выводом команды:

docker service ls
```
[root@node01 ~]# docker service ls
ID             NAME                                MODE         REPLICAS   IMAGE                                          PORTS
w9ks265rmljk   swarm_monitoring_alertmanager       replicated   1/1        stefanprodan/swarmprom-alertmanager:v0.14.0
r35b4q1aavie   swarm_monitoring_caddy              replicated   1/1        stefanprodan/caddy:latest                      *:3000->3000/tcp, *:9090->9090/tcp, *:9093-9094->9093-9094/tcp
b7sav2zq115t   swarm_monitoring_cadvisor           global       6/5        google/cadvisor:latest
ks4xc0uilutk   swarm_monitoring_dockerd-exporter   global       6/5        stefanprodan/caddy:latest
ifahi7vm7as2   swarm_monitoring_grafana            replicated   1/1        stefanprodan/swarmprom-grafana:5.3.4
v2qlo2ipm1b1   swarm_monitoring_node-exporter      global       6/5        stefanprodan/swarmprom-node-exporter:v0.16.0
47hk03jtccrn   swarm_monitoring_prometheus         replicated   1/1        stefanprodan/swarmprom-prometheus:v2.5.0
6g01xd5ptoob   swarm_monitoring_unsee              replicated   1/1        cloudflare/unsee:v0.8.0
[root@node01 ~]#

```



