# SAOU Service

SAOU afhænger af følgende JNDI ressourcer :
- env/iscrum/saou/properties
- saou-log/logback

Derudover skal SAOU bruge en jdbc connection med navn:
- saouservice-production

Og en connection pool med navn:
- pgs-saouservice-production-pool


**env/iscrum/saou/properties**

På resurcen  er der følgende properties: 
- conmanendpoint = https://conman.addi.dk/3.0/
- opensearchendpoint = http://opensearch.addi.dk/b3.0_4.2/
- nalendpoint = http://webservice.statsbiblioteket.dk/nal/services/json/resolve?openurl=
- vipendpoint = http://openagency.addi.dk/2.24/
- use_proxy = true eller false
- proxy_host = host of proxy
- proxy_port = port of proxy

**saou-log/logback**

Lokationen hvor logback skal finde sin include-fil.

F.eks.:
file:///home/mvs/gf-logs/saou-logback-include.xml

**SAOU_LOGBACK_FILENAME**

Bruges af logback til at definere den fulde sti samt filnavn på log filen ekslusiv suffix.

Eksempel:
- /home/mvs/gf-logs/saou

 Vil danne to filer, en for den normale log og en for errorlog:
- /home/mvs/gf-logs/saou.5-08-21.0.log
- /home/mvs/gf-logs/saou-warn-err.2015-08-21.0.log
