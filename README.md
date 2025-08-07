# projeto_speedtest
Servidor de Teste de Velocidade (LibreSpeed) com Docker e Acesso IPv6
Servidor de Teste de Velocidade (LibreSpeed) com Docker e Acesso IPv6
Implantação de uma aplicação de speed test auto-hospedada, superando desafios de rede com CGNAT através de uma solução ponta-a-ponta com IPv6.

🎯 Objetivo do Projeto
O objetivo deste projeto foi implantar uma aplicação web funcional (LibreSpeed) utilizando Docker Compose, aprofundando os conhecimentos em orquestração de contêineres. O principal desafio era garantir que a aplicação, hospedada numa rede doméstica sob CGNAT, ficasse publicamente acessível, o que exigiu a implementação de uma solução baseada em IPv6.

✨ Principais Features e Competências Demonstradas
Implantação com Docker Compose: Orquestração de um serviço a partir de uma imagem pública, definindo redes, portas e variáveis de ambiente num único ficheiro de configuração.

Resolução de Problemas de Rede (CGNAT e IPv6): Diagnóstico de limitações de rede (CGNAT) e implementação de uma solução funcional utilizando endereçamento e mapeamento de portas IPv6.

Configuração de Firewall (firewalld): Gestão de regras de firewall num ambiente Linux para permitir tráfego externo de forma segura para um serviço específico.

Análise e Depuração de Contêineres: Utilização de docker logs e curl para diagnosticar problemas de conectividade (ERR_CONNECTION_RESET) e identificar o comportamento interno da aplicação.

Gestão de Imagens Públicas: Utilização de imagens de contêineres do repositório público ghcr.io.

🛠️ Tecnologias Utilizadas
Docker & Docker Compose: Para conteinerização e orquestração.

LibreSpeed: Aplicação de teste de velocidade de código aberto.

BigLinux (Host OS): Sistema anfitrião onde o Docker foi executado.

firewalld: Ferramenta de gestão de firewall do host.

Rede: Foco em configuração e exposição de serviços via IPv6.

🚀 O Desafio: Acesso Externo em Rede com CGNAT
A maior dificuldade deste projeto foi expor o serviço à internet, uma vez que a rede do host opera sob um CGNAT (Carrier-Grade NAT), tornando o endereço IPv4 privado e inacessível externamente.

A solução foi construir toda a stack de conectividade utilizando IPv6:

O docker-compose.yml foi configurado para mapear a porta do contêiner explicitamente para a interface IPv6 do host, utilizando a sintaxe "[::]:[porta_host]:[porta_container]".

Uma regra foi adicionada ao firewalld do host para permitir tráfego de entrada na porta TCP designada, garantindo que os pedidos externos não fossem bloqueados.

🕵️ O Processo de Depuração (Troubleshooting)
Durante a implantação, um erro ERR_CONNECTION_RESET indicou um problema para além do firewall. A investigação seguiu um fluxo profissional:

Análise de Logs: O comando docker logs foi utilizado para inspecionar a saída do contêiner.

Descoberta: Os logs revelaram que, por design da imagem, o servidor web interno (Apache) alterava a sua porta de escuta padrão de 80 para 8080.

Correção: O docker-compose.yml foi corrigido para mapear a porta externa para a porta interna correta (8080), alinhando a infraestrutura (Docker) com o comportamento real da aplicação.

Este processo demonstrou a importância da leitura de logs para uma depuração eficaz.

📄 Configuração Final (docker-compose.yml)
YAML

services:
  librespeed:
    image: ghcr.io/librespeed/speedtest:latest
    container_name: meu_speedtest
    ports:
      # Mapeamento explícito para IPv6 e para a porta interna correta (8080)
      - "[::]:8082:8080"
    environment:
      - TZ=America/Sao_Paulo
    restart: unless-stopped

    
✅ Resultado Final
Um servidor de teste de velocidade totalmente funcional, implantado via Docker Compose e acessível publicamente através de um endereço IPv6, validando uma solução completa para um desafio de infraestrutura do mundo real.



<img width="600 height="1200" alt="unnamed" src="https://github.com/user-attachments/assets/826e9055-276b-4648-b2a7-648955cba5aa" />

