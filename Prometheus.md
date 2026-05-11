<h1>Exercice 1 :</h1>

docker pull prom/prometheus:latest

docker run -d --name prometheus -p 9090:9090 prom/prometheus:latest

http://localhost:9090
<img width="1911" height="422" alt="image" src="https://github.com/user-attachments/assets/f436aa12-f646-49aa-910c-cf2237de5ee1" />

docker logs prometheus
<img width="1208" height="25" alt="image" src="https://github.com/user-attachments/assets/3fba8d41-ce38-4b7c-8620-878b1bd7a53d" />

<h1>Exercice 2 : </h1>

docker rm -f prometheus

nano prometheus.yml
CONF :

global:
  scrape_interval: 10s
  external_labels:
    environment: lab

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']


lance le docker : docker run -d `
  --name prometheus `
  -p 9090:9090 `
  -v ${PWD}/prometheus.yml:/etc/prometheus/prometheus.yml `
  prom/prometheus:latest `
  --config.file=/etc/prometheus/prometheus.yml `
  --web.enable-lifecycle

localhost:9090 sur le navigateur 
<img width="1916" height="541" alt="image" src="https://github.com/user-attachments/assets/99317b51-f8b9-4802-b62a-825fbc674811" />

modifie le yml
<img width="397" height="200" alt="image" src="https://github.com/user-attachments/assets/baa0ada9-715d-494b-b1ed-1a789d89f7eb" />

<h1>Exercice 3 : </h1>

docker run -d --name node-exporter -p 9100:9100 prom/node-exporter:latest

Pour reload sur PowerShell
Invoke-RestMethod -Method Post -Uri http://localhost:9090/-/reload
<img width="1896" height="387" alt="image" src="https://github.com/user-attachments/assets/79e7ce33-cf87-4799-be6b-30f11a7993d4" />

Tapez dans le bowser node_cpu_seconds_total
Table
<img width="1891" height="841" alt="image" src="https://github.com/user-attachments/assets/21329127-07a4-43c7-9b1b-8e8fb2c7e747" />
Graph
<img width="1790" height="822" alt="image" src="https://github.com/user-attachments/assets/b169c1b9-db8a-4716-b934-6afb239c2139" />

<h1>Exercice 4 : </h1>

Créer le fichier JSON
<img width="519" height="461" alt="image" src="https://github.com/user-attachments/assets/ad20ffaf-083e-4674-8572-1c67bf8825ee" />

Modifier le YML
<img width="535" height="448" alt="image" src="https://github.com/user-attachments/assets/60c5cd3b-ede4-4a64-ae72-ffd34b1705bb" />


Relancer :
docker rm -f prometheus

docker run -d `
  --name prometheus `
  -p 9090:9090 `
  -v ${PWD}/prometheus.yml:/etc/prometheus/prometheus.yml `
  -v ${PWD}/targets.json:/etc/prometheus/sd/targets.json `
  prom/prometheus:latest `
  --config.file=/etc/prometheus/prometheus.yml `
  --web.enable-lifecycle

<img width="1897" height="700" alt="image" src="https://github.com/user-attachments/assets/547c7763-098b-4e36-8f8d-69dfa995fb70" />

Quand on retire le port 8000 du JSON et qu'on attend 5sec 
<img width="1899" height="459" alt="image" src="https://github.com/user-attachments/assets/95c923f2-f2a0-424f-8687-e43f9c13b2a1" />

<h1>Exercice 5 : </h1>

Le nouveau fichier api_rules.yml
<img width="548" height="198" alt="image" src="https://github.com/user-attachments/assets/0ff3e906-b073-446b-9fec-2911062eae7d" />

Modifier le prometheus.yml et rajoutez : 
rule_files:
  - '/etc/prometheus/rules/*.yml'

Dans status -> rules
<img width="1876" height="232" alt="image" src="https://github.com/user-attachments/assets/2c8a1993-99d8-4820-a2e1-32682b67e94b" />

Dans la query on mes : job:http_requests:rate5m et cela nous donne ceci car il n'y a pas l'api de lancer
<img width="1902" height="322" alt="image" src="https://github.com/user-attachments/assets/c75163ea-91e2-4069-b534-8fd7551ef6f4" />




