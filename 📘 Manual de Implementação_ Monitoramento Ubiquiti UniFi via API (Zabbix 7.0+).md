# 📘 Monitoramento Ubiquiti UniFi via API no Zabbix 7.0+

Bem-vindo ao repositório do manual de implementação e template para monitoramento de controladoras Ubiquiti UniFi utilizando a API oficial, compatível com Zabbix 7.0+.

Este guia e template foram criados para ajudar administradores de sistemas e entusiastas de monitoramento a configurar um sistema robusto e eficiente, resolvendo problemas comuns de compatibilidade de JavaScript (`ReferenceError: host is not defined`) encontrados na versão 7.0+ do Zabbix.

## Conteúdo do Repositório

*   **`Ubiquiti_UniFi_Zabbix_API_Monitoring.md`**: Manual detalhado de implementação passo a passo.
*   **`template_ubiquiti_unifi_sanitized_generic.yaml`**: Template Zabbix exportado (YAML) pronto para importação, com macros para dados sensíveis.

## Como Utilizar o Template Zabbix

Para utilizar o template `template_ubiquiti_unifi_sanitized_generic.yaml` no seu ambiente Zabbix, siga os passos abaixo:

### 1. Importar o Template

1.  Aceda à interface web do Zabbix.
2.  Navegue até `Configuration` > `Templates`.
3.  No canto superior direito, clique em `Import`.
4.  Clique em `Choose file` e selecione o arquivo `template_ubiquiti_unifi_sanitized_generic.yaml` que você descarregou deste repositório.
5.  Certifique-se de que as opções de importação estão corretas (geralmente as predefinições são suficientes).
6.  Clique em `Import`.

### 2. Configurar Macros Essenciais

O template utiliza macros para armazenar informações sensíveis e específicas do seu ambiente UniFi. Após a importação, você **DEVE** configurar estas macros no seu **Host** ou no **Template** (se quiser que se aplique a todos os hosts vinculados a ele).

1.  Navegue até `Configuration` > `Hosts` (ou `Templates` se for configurar no template).
2.  Selecione o Host (ou Template) onde o `Template Ubiquiti UniFi Pro AP and Switch by API` está vinculado.
3.  Clique na aba `Macros`.
4.  Adicione as seguintes macros, substituindo os valores pelos dados reais da sua controladora UniFi:

    | Macro             | Descrição                                         | Exemplo de Valor (Fictício) |
    | :---------------- | :------------------------------------------------ | :-------------------------- |
    | `{$UNIFI.HOST}`   | IP ou FQDN da sua controladora UniFi.             | `192.168.1.100`             |
    | `{$UNIFI.PORT}`   | Porta da API UniFi (geralmente 8443 ou 443).      | `8443`                      |
    | `{$UNIFI.USER}`   | Nome de utilizador da conta de serviço da API UniFi (criada no passo 1.1 do manual). | `zabbix_user`               |
    | `{$UNIFI.PASS}`   | Senha da conta de serviço da API UniFi.           | `SuaSenhaSecretaAqui!`      |

5.  Clique em `Update` para salvar as macros.

### 3. Vincular o Template a um Host

Se ainda não o fez, vincule o template ao seu host Zabbix que representa a controladora UniFi:

1.  Navegue até `Configuration` > `Hosts`.
2.  Selecione o host da sua controladora UniFi.
3.  Clique na aba `Templates`.
4.  Adicione o `Template Ubiquiti UniFi Pro AP and Switch by API`.
5.  Clique em `Update`.

### 4. Verificar o Monitoramento

Após configurar as macros e vincular o template, o Zabbix começará a coletar dados. Você pode forçar a descoberta e verificar os dados:

1.  No Host, vá a `Discovery rules`.
2.  Marque a regra `Descoberta de Dispositivos UniFi` e clique em `Execute now`.
3.  Vá a `Monitoring` > `Latest data` e valide se os dados dos dispositivos UniFi estão a chegar.

## Como Contribuir

Sinta-se à vontade para abrir `issues` para sugestões, melhorias ou dúvidas. Pull requests são sempre bem-vindos!

## Licença

Este projeto está licenciado sob a Licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

--- 

**Autor**: Luisbnc
**Data**: 16 de Março de 2026
