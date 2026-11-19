# Escalabilidade e Componentes de Infraestrutura

Aqui você aprende a lidar com múltiplos usuários e evitar que o sistema caia sob alta carga.
  * **Load Balancers (Balanceadores de Carga)**: Distribuem o tráfego entre múltiplos servidores (algoritmos como Round-Robin, Least Connections).
  * **Caching (Cache)**: Uso de ferramentas como Redis ou Memcached para guardar dados acessados com frequência na memória RAM, aliviando o banco de dados.
  * **CDNs (Content Delivery Networks)**: Distribuição de arquivos estáticos (imagens, vídeos, JS/CSS) globalmente para reduzir a latência (ex: Cloudflare, AWS CloudFront).
  * **Rate Limiting**: Proteção de APIs contra abusos e ataques de negação de serviço (DoS/DDoS) limitando requisições por usuário.
