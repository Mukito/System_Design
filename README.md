# System_Design
Repositório para estudo de design de system - Inicio

## Fase 1: Fundamentos de Rede e Comunicação
Antes de desenhar sistemas complexos, é preciso entender como os computadores conversam entre si na internet.
  * **Protocolos da Web**: Entenda a diferença e o funcionamento de HTTP/HTTPS, TCP vs. UDP e DNS (Domain Name System).
  * **APIs e Estilos de Comunicação**:
    * **RESTful APIs**: O padrão mais comum baseado em recursos e verbos HTTP.
    * **gRPC e WebSockets**: Para comunicação em tempo real ou de alta performance (binária/streaming).
  
  * **DNS e Roteamento**: Como o navegador encontra um servidor e como o tráfego é direcionado.

## Fase 2: Bancos de Dados e Armazenamento
Saber onde e como os dados são guardados é o coração de qualquer projeto.
  * **SQL vs. NoSQL**:
    * Relacionais (PostgreSQL, MySQL): Consistência estrita, tabelas e relações (ACID).
    * Não Relacionais (MongoDB, Redis, Cassandra): Flexibilidade, alta velocidade, dados chave-valor ou documentos.
  * **Indexação e Transações**: Como funcionam os índices de banco de dados para acelerar buscas e o que são transações ACID e o Teorema CAP (Consistência, Disponibilidade e Tolerância a Particionamento).
  * **Replicação e Sharding**: Como escalar bancos de dados horizontalmente quando o volume de dados cresce demais.

## Fase 3: Escalabilidade e Componentes de Infraestrutura
Aqui você aprende a lidar com múltiplos usuários e evitar que o sistema caia sob alta carga.
  * **Load Balancers (Balanceadores de Carga)**: Distribuem o tráfego entre múltiplos servidores (algoritmos como Round-Robin, Least Connections).
  * **Caching (Cache)**: Uso de ferramentas como Redis ou Memcached para guardar dados acessados com frequência na memória RAM, aliviando o banco de dados.
  * **CDNs (Content Delivery Networks)**: Distribuição de arquivos estáticos (imagens, vídeos, JS/CSS) globalmente para reduzir a latência (ex: Cloudflare, AWS CloudFront).
  * **Rate Limiting**: Proteção de APIs contra abusos e ataques de negação de serviço (DoS/DDoS) limitando requisições por usuário.

## Fase 4: Mensageria e Processamento Assíncrono
Sistemas modernos frequentemente desacoplam tarefas para não travar a experiência do usuário.
  * **Message Queues e Pub/Sub**: Filas de mensagens e eventos (como **RabbitMQ, Apache Kafka, AWS SQS**).
  * **Casos de uso**: Envio de e-mails em lote, processamento de vídeos, geração de relatórios pesados em segundo plano.

## Fase 5: Prática e Estudos de Caso (System Design Interviews)
A melhor forma de fixar o conhecimento é resolvendo problemas clássicos de arquitetura. Pratique projetar os seguintes sistemas:
  **1. Encurtador de URLs** (foco em geração de IDs únicos, redirecionamento e volume de leitura).
  **2. Sistema de Feed de Notícias / Twitter** (foco em Fan-out on write vs Fan-out on read).
  **3. Chat em Tempo Real / WhatsApp** (foco em WebSockets e gerenciamento de conexões persistentes).
  **4. Streaming de Vídeo / Netflix** (foco em CDN, armazenamento de arquivos pesados e adaptação de bitrate).

## 📚 Recursos Recomendados
  * **Livro**: Designing Data-Intensive Applications (de Martin Kleppmann) — considerado a "bíblia" do assunto.
  * **Repositório**: System Design Primer (no GitHub, por Alex Xu) — excelente guia comunitário gratuito.
  * **Livros de Entrevista**: System Design Interview – An insider's guide (Volumes 1 e 2) por Alex Xu.


