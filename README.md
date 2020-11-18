# zabbix-prometheus-alertmanager

# Requisitos:
Zabbix versão >= 5.0 

# Instruções de uso:
1) Instale o Template via import no zabbix;
2) Associe o template ao HOST que tenha o IP de acesso ao Alertmanager;
3) Crie os alertas do Alertmanager (rules) adicionando uma linha em labels com o nome de "severity", onde deve conter uma das seguintes opções conforme criticidade do alerta: information,warning,average,high,disaster;

# Exemplo alert rules:
    groups:
       - name: node_alerts
         rules:
         - alert: node_exporterDow
           expr: up{job="node_exporter"} == 0
           for: 1m
           labels:
             severity: high
           annotations:
             summary: Host {{ $labels.instance }} of {{ $labels.job }}


# Observações:
1) Fique atento a porta de acesso do Prometheus, por padrão no template está configurado a porta 9090 para acessar os dados de alerta do Prometheus via HTTP API, mas pode ser alterado a porta em Macros do template.
2) O titulo do alarme que será visualizado no zabbix é o conteúdo configurado em "summary" contido em "annotations" do seu arquivo de rules e em tags de trigger são utilizados os campos: job,alert e instance. 
3) Todo alarme é identificado via regra de descoberta no zabbix, a condição para normalizar é sair do página de alarta do Alertmanager e o prometheus deve estar respondendo a chamada da HTTP API.
4) No zabbix será salvo sempre o último valor que sofreu modificação e não será salvo valor repetido sequencialmente.


Version

    1.0 - initial
