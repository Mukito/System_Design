# DNS e Roteamento

O **DNS (Domain Name System) e o Roteamento** formam a infraestrutura invisível que permite que você digite um endereço amigável no navegador e chegue exatamente ao servidor correto, em milissegundos, independentemente de onde você esteja no mundo.

## 1. A Jornada de uma Requisição: O Fluxo do DNS
Quando você digita [https://api.minhaempresa.com](https://api.minhaempresa.com) no navegador, o sistema precisa transformar esse nome legível em um endereço IP numérico (como 192.0.2.1 ou um IPv6). Esse processo acontece em etapas:
  1. **Cache Local**: O navegador verifica se já conhece o IP daquele domínio (salvo recentemente). Se não encontrar, pergunta ao sistema operacional (que também tem um cache DNS próprio).
  2. **Resolvedor DNS (ISP)**: Se o cache local falhar, o sistema operacional pergunta ao seu Provedor de Internet (ISP) ou a um DNS público (como Google 8.8.8.8 ou Cloudflare 1.1.1.1).
  3. **Servidor Raiz (Root DNS Server)**: Se o resolvedor não tiver a resposta, ele consulta um dos 13 servidores raiz globais, que aponta qual servidor é responsável pelo domínio de topo (ex: .com).
  4. **Servidor TLD (Top-Level Domain)**: O servidor .com direciona o resolvedor para o Servidor Autoritativo específico daquele domínio (geralmente gerenciado pelo serviço de hospedagem ou registrador, como AWS Route 53 ou Cloudflare).
  5. **Resposta Final**: O servidor autoritativo retorna o endereço IP exato para o seu computador, que armazena a resposta em cache pelo tempo determinado (TTL - Time to Live) e inicia a conexão.

## 2. O Papel do DNS no System Design (Escalabilidade e Alta Disponibilidade)
Para arquitetos de software, o DNS não serve apenas para traduzir nomes; ele é a primeira linha de defesa e balanceamento de carga de um sistema global:
  * **GeoDNS (Roteamento Geográfico)**: O DNS pode ser configurado para analisar de qual país ou região a consulta veio. Um usuário na Europa recebe o IP do servidor em Frankfurt, enquanto um usuário no Brasil recebe o IP de São Paulo, reduzindo drasticamente a latência.
  * **Balanceamento de Carga via DNS (Round-Robin)**: Um único domínio pode retornar múltiplos IPs. O DNS vai alternando a ordem dos IPs entregues aos usuários, distribuindo o tráfego inicial entre vários servidores.
  * **Failover e Tolerância a Falhas**: Se o data center principal cair, o serviço de DNS pode detectar a falha automaticamente e atualizar os registros para apontar o tráfego para um data center de backup (Disaster Recovery).

## 3. Roteamento de Tráfego: Como os Dados Viajam
Uma vez que o navegador descobre o IP através do DNS, os pacotes de dados precisam navegar fisicamente pela internet. É aqui que entram os roteadores e os protocolos de rede:
  * **Endereçamento IP e Subredes**: Cada máquina na internet possui um endereço IP único. Os roteadores analisam o endereço de destino de cada pacote de dados e decidem qual é o "próximo salto" (next hop) mais eficiente.
  * **BGP (Border Gateway Protocol)**: É o protocolo que cola a internet inteira. Ele gerencia como diferentes redes autônomas (como provedores de internet, grandes corporações e CDNs) trocam informações sobre quais rotas estão disponíveis para alcançar determinados IPs.
  * **Anycast Routing**: Uma técnica avançada usada por CDNs (como Cloudflare) e servidores DNS onde vários servidores ao redor do mundo anunciam exatamente o mesmo endereço IP. Os roteadores globais da internet encaminham automaticamente o pacote do usuário para o nó físico geograficamente mais próximo.
