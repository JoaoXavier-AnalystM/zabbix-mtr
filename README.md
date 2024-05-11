# Zabbix My traceroute Network Monitoring

## Introdução

Este modelo foi construído no Zabbix 6.0.x, 6.4.x e RHEL 9 amd64/arm64.

## Instalação

1. Copie o script [mtr-zbx.sh](mtr-zbx.sh) para pasta **ExternalScripts**  do seu Zabbix Server ou Zabbix Proxy. De permissão de execução para o script: `chmod +x mtr.sh` (As verificações externas são executadas com usuário `zabbix` que é criado na instalação do Zabbix) e por fim adicione permissão de execução para o arquivo.

Juntando todos os passos temos:

`#cp /tmp/mtr-zbx.sh /usr/lib/zabbix/externalscripts/`

`#chown zabbix:zabbix /usr/lib/zabbix/externalscripts/*`

`#chmod +x /usr/lib/zabbix/externalscripts/mtr-zbx.sh`

2. Validar execução e permissão user `zabbix`

`sudo -u zabbix ./mtr-zbx.sh`

![alt text](https://github.com/JoaoXavier-AnalystM/zabbix-mtr/blob/main/images/image-2.png)

3. Importe o Template [Template MTR](zbx_mtr_templates.yaml) 

## Configurações

Ao incluir o novo host, link o mesmo a Template MTR
![ADD Host](https://github.com/JoaoXavier-AnalystM/zabbix-mtr/blob/main/images/image-1.png)

Depois vá ate o campo **MACROS** e adiciono o IP a qual deseja realizar a verificação:

![Inc IP](https://github.com/JoaoXavier-AnalystM/zabbix-mtr/blob/main/images/image-3.png)

Apos os procedimentos aguarde as coletas serem populadas.


### Script
> Primeiro, o script atribui o caminho do comando mtr ao variável MTR usando o comando which. 

> Em seguida, o script atribui o endereço IP passado como argumento para a variável IP.

Depois disso, o script executa o comando MTR com algumas opções:

>- -r: Mostra os resultados na forma de relatório.
>- -c3: Realiza três ciclos de teste.
>- -w: Habilita a saída no formato JSON.
>- -b: Ativa o modo de execução contínua, útil para monitoramento.
>- -p: Usa ICMP em vez de UDP para enviar pacotes.
>- -j: Especifica o IP de destino para o teste de traceroute.

## Notas
Este modelo foi testado no Zabbix 6.0.x, 6.4.x e RHEL 9 amd64/arm64. O arquivo YAML fornecido é exportado do Zabbix 6.4.11.

Este modelo cria um item CORE, que executa o script e recebe o retorno do JSON. A partir daí temos uma regra de descoberta de baixo nível(LDD), que descobre 4 itens  prototypes:

>- Nome de cada salto(Hop) -  MTR Hop
>- RTT médio (ms) de cada salto - Average RTT(ms)
>- % de perda de ICMP de cada salto - ICMP Loss%
>- Tempo que levou para o último pacote ser recebido em determinado salto - Last(ms)

### Ajustes

Parâmetro Timeout no arquivo conf Server/proxy caso necessário:

“Por default o Zabbix aguarda uma coleta por 3 segundos, se não houver um retorno, este item se tornará como Não suportado (Not Supported)”

![alt text](https://github.com/JoaoXavier-AnalystM/zabbix-mtr/blob/main/images/image.png)

## Próximas atualizações

- Triggers prototypes
- Dashboard Grafana

## Dados Coletados
![alt text](https://github.com/JoaoXavier-AnalystM/zabbix-mtr/blob/main/images/image-4.png)

## DOCS

Claro, aqui estão os links em formato Markdown:

[Zabbix External Check](https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/external)

[Zabbix Agent Item](https://www.zabbix.com/documentation/current/en/manual/config/items/itemtypes/zabbix_agent)