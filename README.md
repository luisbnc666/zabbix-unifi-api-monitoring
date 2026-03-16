# 📘 Monitoramento Ubiquiti UniFi via API no Zabbix 7.0+

Bem-vindo ao repositório do manual de implementação para monitoramento de controladoras Ubiquiti UniFi utilizando a API oficial, compatível com Zabbix 7.0+.

Este guia foi criado para ajudar administradores de sistemas e entusiastas de monitoramento a configurar um sistema robusto e eficiente, resolvendo problemas comuns de compatibilidade de JavaScript (`ReferenceError: host is not defined`) encontrados na versão 7.0+ do Zabbix.

Este projeto nasceu da necessidade de monitorar de forma eficaz os dispositivos **Ubiquiti UniFi U6 Pro** no Zabbix 7.0+. Após uma extensa pesquisa, constatou-se a **ausência de templates funcionais** tanto em canais oficiais quanto na vasta comunidade Zabbix/Ubiquiti, que pudessem atender às especificidades e à robustez exigidas por esses equipamentos.

Diante desse cenário, desenvolvi este manual e template, focando na integração via **API oficial da UniFi**. A experiência demonstrou que a abordagem via API oferece uma **performance e granularidade de dados significativamente superiores** em comparação com o monitoramento tradicional via SNMP, que muitas vezes se mostra limitado ou inconsistente para as controladoras UniFi e seus dispositivos mais recentes.

Este repositório visa preencher essa lacuna, oferecendo uma solução robusta e testada para a comunidade, permitindo um monitoramento detalhado e confiável dos seus Access Points UniFi U6 Pro e outros dispositivos compatíveis, superando as limitações encontradas nas abordagens existentes.


## Conteúdo do Manual

*   **[Manual de Implementação: Monitoramento Ubiquiti UniFi via API (Zabbix 7.0+)](Ubiquiti_UniFi_Zabbix_API_Monitoring.md)**: Documento detalhado com o passo a passo para configurar o monitoramento.
*   **[Template Zabbix UniFi API (YAML)](template_ubiquiti_unifi_corrected.yaml)**: O template Zabbix exportado em formato YAML, pronto para importação.

O manual `Ubiquiti_UniFi_Zabbix_API_Monitoring.md` abrange os seguintes tópicos:

*   **Configuração do Controlador Ubiquiti UniFi**: Criação de um utilizador de serviço com permissões adequadas.
*   **Configuração do Template no Zabbix**: Criação de um template reutilizável e configuração de macros essenciais para autenticação.
*   **Regra de Descoberta (LLD) e Script Corrigido**: Detalhes sobre a regra de descoberta de baixo nível e o script JavaScript refatorizado para Zabbix 7.0+.
*   **Protótipos de Itens e Gatilhos (Triggers)**: Criação de protótipos para itens de monitoramento e gatilhos de alerta.
*   **Implementação no Host**: Como vincular o template a um host e forçar a coleta de dados.
*   **Saneamento do Dashboard**: Dicas para evitar falsos positivos e manter o dashboard limpo.

## Como Contribuir

Sinta-se à vontade para abrir `issues` para sugestões, melhorias ou dúvidas. Pull requests são sempre bem-vindos!

## Licença

Este projeto está licenciado sob a Licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

--- 

**Autor**: Luisbnc
**Data**: 16 de Março de 2026
