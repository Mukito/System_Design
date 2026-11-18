# Protocolos da Web
Os **Protocolos da Web** formam a fundação sobre a qual toda a internet e qualquer arquitetura de software moderna são construídas. Em System Design, dominar esses protocolos é essencial para decidir como os clientes (browsers, apps mobile) vão conversar com os seus servidores, e como os próprios microsserviços se comunicam entre si.

Vamos detalhar os quatro pilares fundamentais dessa camada:

### 1. DNS (Domain Name System) — O "Guia Telefônico" da Internet
Antes de qualquer requisição HTTP acontecer, o sistema precisa descobrir onde o servidor está localizado.
  * **O que faz**: Traduz nomes legíveis por humanos (ex: `api.minhaempresa.com`) em endereços IP numéricos (ex: `192.0.2.1` ou um IPv6).
  * **Como funciona (Resumo)**: O navegador consulta o cache local, depois o roteador, o ISP (Provedor de Internet) e, se necessário, os servidores raiz até encontrar o IP correspondente.
  * **Importância no System Design**:
    * **Latência**: Uma consulta DNS mal otimizada adiciona milissegundos preciosos à requisição inicial.
    * **GeoDNS / Load Balancing**: O DNS pode ser configurado para retornar IPs diferentes dependendo da localização geográfica do usuário (redirecionando um brasileiro para o data center de São Paulo e um europeu para Frankfurt).

### 2. TCP vs. UDP — As Regras de Transporte
Abaixo do HTTP/HTTPS estão os protocolos de transporte que definem como os pacotes de dados viajam pela rede.
  * **TCP (Transmission Control Protocol)**:
    * Características: Orientado à conexão, confiável, garante que os pacotes cheguem na ordem certa e sem perdas (se um pacote se perde, ele é reenviado).
    * Uso: É a base do **HTTP**, requisições de API, bancos de dados e transferências de arquivos onde a integridade dos dados é absoluta.
    * Preço: Possui uma sobrecarga (overhead) maior devido ao 3-way handshake (processo inicial de estabelecimento de conexão).

  * **UDP (User Datagram Protocol)**:
    * Características: Sem conexão, rápido, leve, mas **não garante** entrega nem ordem (pacotes podem se perder no caminho).
    * Uso: Streaming de vídeo ao vivo, chamadas de voz (VoIP), jogos online e protocolos modernos como **HTTP/3 (QUIC)**, onde perder um frame de vídeo é melhor do que travar a transmissão esperando o pacote reenviado.

### 3. HTTP / HTTPS — A Linguagem da Web
O **HTTP** (Hypertext Transfer Protocol) é o protocolo de camada de aplicação usado para transferir hipertexto na web. Ele opera no modelo de **Requisição e Resposta** (Client-Server).

  * **Anatomia de uma Requisição**:
    * **Métodos (Verbos)**: GET (buscar dados), POST (criar dados), PUT/PATCH (atualizar dados) e DELETE (remover dados).
    * **Status Codes**:
      * 2xx (Sucesso - ex: 200 OK, 201 Created)
      * 4xx (Erro do Cliente - ex: 400 Bad Request, 401 Unauthorized, 404 Not Found)
      * 5xx (Erro do Servidor - ex: 500 Internal Server Error, 502 Bad Gateway)

  * **Evolução (HTTP/1.1 vs HTTP/2 vs HTTP/3)**:
    * HTTP/1.1: Uma requisição por vez por conexão TCP (ou conexões paralelas limitadas). Sofria com o problema de Head-of-Line Blocking.
    * HTTP/2: Permite multiplexação (várias requisições simultâneas na mesma conexão TCP) e compressão de cabeçalhos.
    * HTTP/3: Substitui o TCP pelo QUIC (baseado em UDP) para eliminar gargalos de conexão e melhorar a performance em redes instáveis (como 4G/5G).

  * **HTTPS**: É o HTTP puro encapsulado em uma camada de segurança **TLS/SSL**. Criptografa os dados em trânsito, garantindo privacidade e integridade (ninguém no meio do caminho pode ler senhas ou tokens).

### 4. Alternativas Modernas: WebSockets e gRPC
Embora o HTTP/REST seja o padrão para a maioria das APIs, cenários específicos exigem protocolos especializados:

  * **WebSockets**:
    * O que é: Permite uma conexão persistente e bidirecional (full-duplex) sobre TCP. O servidor pode enviar dados para o cliente ativamente sem que o cliente precise pedir (polling).
    * Uso: Chats em tempo real, dashboards financeiros (preço de ações oscilando), jogos e notificações push.
  * **gRPC**:
    * O que é: Um framework de RPC (Remote Procedure Call) de alta performance criado pelo Google. Usa HTTP/2 por baixo dos panos e serializa os dados usando **Protocol Buffers (Protobuf)** em vez de JSON.
    * Vantagem: Mensagens binárias muito menores e mais rápidas de processar. É o padrão de mercado para comunicação interna entre **microsserviços**.
   

