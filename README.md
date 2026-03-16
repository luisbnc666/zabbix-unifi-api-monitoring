# 📘 Monitoramento Ubiquiti UniFi via API no Zabbix 7.0+

Bem-vindo ao repositório do manual de implementação para monitoramento de controladoras Ubiquiti UniFi utilizando a API oficial, compatível com Zabbix 7.0+.

Este guia foi criado para ajudar administradores de sistemas e entusiastas de monitoramento a configurar um sistema robusto e eficiente, resolvendo problemas comuns de compatibilidade de JavaScript (`ReferenceError: host is not defined`) encontrados na versão 7.0+ do Zabbix.

## Conteúdo do Manual

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
