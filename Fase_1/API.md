# APIs e os estilos de comunicação

As APIs e os estilos de comunicação definem como os diferentes serviços e clientes trocam dados entre si. Escolher o estilo correto afeta diretamente a performance, o consumo de bateria (no mobile) e a complexidade da sua arquitetura.

Vamos detalhar os três principais estilos mencionados no roadmap e quando utilizar cada um no projeto de um sistema.

## 1. REST (Representational State Transfer) — O Padrão da Indústria
O REST é um estilo arquitetural (e não um protocolo rígido) construído sobre os fundamentos do HTTP. Ele trata tudo na sua aplicação como um recurso (ex: /usuarios, /produtos/123), manipulado através dos verbos HTTP.
  * **Características Principais**:
    * **Stateless (Sem Estado)**: Cada requisição do cliente para o servidor deve conter todas as informações necessárias para entendê-la e processá-la. O servidor não guarda o contexto da sessão do cliente nas requisições.
    * **Cacheável**: Respostas podem ser explicitamente marcadas como cacheáveis para reduzir o tráfego de rede.
    * **Formato Padrão**: Geralmente usa JSON para o payload de dados.

  * **Vantagens**: Curva de aprendizado baixa, amplamente suportado por qualquer linguagem/framework, fácil de debugar (você pode testar no navegador ou Postman).
  * **Desvantagens**:
    * Over-fetching (receber dados demais que você não vai usar) ou Under-fetching (precisar fazer múltiplas requisições para montar uma tela).
    * Ineficiente para comunicação interna de alta frequência entre microsserviços devido ao tamanho do texto (JSON) e overhead do HTTP/1.1.

## 2. gRPC — Alta Performance para Microsserviços
Criado pelo Google, o gRPC é um framework moderno de RPC (Remote Procedure Call). Ele permite que um programa chame funções executadas em outro computador (ou servidor) como se fossem locais.
  * **Como funciona por baixo dos panos**:
    * Utiliza HTTP/2 como transporte (permitindo multiplexação, streaming bidirecional e cabeçalhos compactados).
    * Usa Protocol Buffers (Protobuf) para a serialização. Em vez de JSON (que é texto legível), os dados são transformados em binários compactos.
  * **Vantagens**:
    * Extremamente rápido e consome menos largura de banda (payloads até 10x menores que JSON).
    * Geração automática de código cliente/servidor fortemente tipado a partir de arquivos .proto.
  * **Quando usar**: Comunicação síncrona interna entre microsserviços onde latência mínima e alta taxa de transferência (throughput) são cruciais.

## 3. WebSockets — Comunicação em Tempo Real Bidirecional
O HTTP tradicional funciona no modelo "bate e volta" (request-response): o cliente pergunta, o servidor responde e a conexão fecha (ou fica em espera). Para cenários onde o servidor precisa enviar dados para o cliente assim que eles acontecem, o HTTP tradicional falha (exigindo truques ineficientes como polling).
  * **Como funciona**: O cliente faz uma requisição HTTP inicial pedindo um "handshake" de WebSocket. A conexão é atualizada (upgrade) e passa a ser uma **conexão TCP persistente e bidirecional (full-duplex)**.
  * **Vantagens**: Permite que dados fluam livremente em ambas as direções a qualquer momento, com baixíssima latência e sem o overhead de reabrir cabeçalhos HTTP a cada mensagem.
  * **Quando usar**: Chats de mensagens, dashboards de mercado financeiro em tempo real, jogos multiplayer baseados em navegador/app, e notificações push instantâneas.



### 📊 Comparativo de APIs e Estilos de Comunicação

| Critério | REST (JSON) | gRPC (Protobuf) | WebSockets |
| :--- | :--- | :--- | :--- |
| **Público-alvo Principal** | Clientes externos (Web, Mobile, Terceiros) | Comunicação interna entre Microsserviços | Aplicações que exigem tempo real |
| **Protocolo Base** | HTTP/1.1 ou HTTP/2 | HTTP/2 | TCP (via *upgrade* HTTP) |
| **Formato dos Dados** | Texto legível (JSON / XML) | Binário estrito (Protocol Buffers) | Texto ou Binário livre |
| **Padrão de Fluxo** | Request / Response (Unidirecional) | Request / Response e Streaming | Bidirecional contínuo (*Full-duplex*) |
| **Latência e Performance** | Moderada (overhead de texto e cabeçalhos) | **Muito Baixa** (payloads compactos) | **Baixíssima** (sem overhead de conexão) |
| **Suporte Nativo em Browsers**| Total (via `fetch` / `axios`) | Limitado (requer proxy *gRPC-Web*) | Total (via API nativa `WebSocket`) |
| **Cacheamento HTTP** | Fácil e nativo (GETs são cacheáveis) | Difícil (requisições POST/binárias) | Não aplicável (fluxo contínuo) |
| **Facilidade de Debug** | Alta (legível por humanos, cURL/Postman) | Média (exige ferramentas específicas) | Média (inspeção de frames no DevTools) |
| **Casos de Uso Ideais** | APIs públicas, CRUDs, sistemas web padrão | Alta carga interna, IoT, baixa latência | Chats, jogos multiplayer, dashboards ao vivo |

> **💡 Regra de Ouro para System Design:** Comece com **REST** para APIs públicas e integrações simples; migre para **gRPC** quando o gargalo de rede entre microsserviços internos exigir performance máxima; adote **WebSockets** apenas quando o servidor precisar enviar dados ativamente ao cliente em tempo real.





