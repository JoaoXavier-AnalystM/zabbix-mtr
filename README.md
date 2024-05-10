# Zabbix My traceroute Network Monitoring
MTR - ZBX

## Introdução

Este modelo foi construído no Zabbix 6.0.x, 6.4.x e RHEL 9 amd64/arm64.

## Instalação

1. Copie o script [mtr-zbx.sh](mtr-zbx.sh) para pasta **ExternalScripts**  do seu Zabbix Server ou Zabbix Proxy. De permisão de execução para o script: `chmod +x mtr.sh` (As verificações externas são executadas com usuário `zabbix` que é criado na instalação do Zabbix) e por fim adicione permissão de execução para o arquivo.

Juntando todos os passos temos:


`#cp /tmp/mtr-zbx.sh /usr/lib/zabbix/externalscripts/`

`#chown zabbix:zabbix /usr/lib/zabbix/externalscripts/*`

`#chmod +x /usr/lib/zabbix/externalscripts/mtr-zbx.sh`


2. Validar execução e permissão user `zabbix`

`sudo -u zabbix ./mtr-zbx.sh`

![alt text](image-2.png)


3. Importe o Template [Template MTR](zbx_mtr_templates.yaml) 

## Configurações

Ao incluir o novo host, link o mesmo a Template MTR
![ADD Host](image-1.png)

Depois vá ate o campo **MACROS** e adiciono o IP a qual deseja realziar a verificação:

![Inc IP](image-3.png)

Apos os procedimentos aguarde as coletas serem populadas.

## Notas

Este modelo foi testado no Zabbix 6.0.x, 6.4.x e RHEL 9 amd64/arm64. O arquivo YAML fornecido é exportado do Zabbix 6.4.11.


Este modelo cria um item CORE, que executa o script e recebe o retorno do JSON. A partir daí temos uma regra de descoberta de baixo nível(LDD), que descobre 4 itens  prototypes:


>- Nome de cada salto(Hop) -  MTR Hop
>- RTT médio (ms) de cada salto - Average RTT(ms)
>- % de perda de ICMP de cada salto - ICMP Loss%
>- Tempo que levou para o último pacote ser recebido em determinado salto - Last(ms)




## Dados Coletados
![Latest data](image.png)