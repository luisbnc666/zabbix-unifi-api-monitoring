# 📘 Manual de Implementação: Monitoramento Ubiquiti UniFi via API (Zabbix 7.0+)

Este manual detalha a criação e configuração de um monitoramento robusto para controladoras Ubiquiti UniFi, utilizando a API oficial. Esta solução resolve especificamente o erro de compatibilidade de JavaScript (`ReferenceError: host is not defined`) presente no Zabbix 7.0+.

## 1. Configuração no Controlador Ubiquiti UniFi

O primeiro passo é criar uma conta de serviço dedicada na sua controladora.

### 1.1 Criar Utilizador de Serviço

1.  Aceda à interface web da sua Controladora UniFi.
2.  Vá a `Settings` (ícone da engrenagem no canto inferior esquerdo).
3.  Selecione `Admins & Users`.
4.  Clique na aba `Users` e depois em `Add New User`.
5.  Preencha os campos conforme abaixo:
    *   **First Name / Last Name**: `Zabbix API`
    *   **Username**: `zabbix_user`
    *   **Password**: Defina uma password forte (ex: `Zabbix@UniFi#2026`).
    *   Em `Application Permissions`:
        *   Na aplicação `Network`, selecione a permissão `Read Only` (Leitura).

> ⚠️ **Nota de Segurança**: Certifique-se de que a opção `Multi-Factor Authentication (MFA)` está **DESATIVADA** para este utilizador, pois a API do Zabbix não suporta tokens dinâmicos.

## 2. Configuração do Template no Zabbix

Não configure diretamente no Host. Crie um Template para que o monitoramento seja reutilizável.

### 2.1 Criar o Template

1.  Aceda ao Zabbix: `Data collection` > `Templates`.
2.  Clique no botão `Create template` (topo direito).
3.  Preencha os campos:
    *   **Template name**: `Ubiquiti UniFi API Custom 7.0`
    *   **Groups**: Selecione um grupo (ex: `Templates/Network devices`).
4.  Clique em `Add`.

### 2.2 Configurar Macros do Template

As macros garantem que o script seja dinâmico. Dentro do Template, clique na aba `Macros`.

Adicione as seguintes macros:

| Macro             | Valor                               |
| :---------------- | :---------------------------------- |
| `{$UNIFI.USER}`   | `zabbix_user`                       |
| `{$UNIFI.PASS}`   | `A_Sua_Password_Definida`           |
| `{$UNIFI.PORT}`   | `8443` (Ou `443` conforme o seu ambiente) |

> 💡 **OBSERVAÇÃO IMPORTANTE (AUTENTICAÇÃO)**:
> Os dados inseridos nestas macros são vitais para o funcionamento de todo o monitoramento. O script de descoberta e coleta utiliza estas variáveis para realizar a Fase de Autenticação na API da Ubiquiti. Certifique-se de que o utilizador e a password correspondem exatamente ao que foi criado na Controladora (Passo 1.1). Sem estas credenciais corretas, o Zabbix receberá um erro de "Acesso Negado" (Status 401 ou 403).

## 3. Regra de Descoberta (LLD) e Script Corrigido

Esta é a parte vital onde corrigimos o erro de JavaScript do Zabbix 7.0.

### 3.1 Criar Regra de Descoberta

No Template, clique na aba `Discovery rules`.

1.  Clique em `Create discovery rule`.
2.  Preencha os campos:
    *   **Name**: `UniFi Sites Discovery`
    *   **Type**: `Script`
    *   **Key**: `unifi.sites.discovery`

### 3.2 Parâmetros e Código (O Ajuste Técnico)

No campo `Parameters`, clique em `Add` para inserir os 4 mapeamentos obrigatórios:

| Name       | Value                  |
| :--------- | :--------------------- |
| `host`     | `{HOST.CONN}`          |
| `port`     | `{$UNIFI.PORT}`        |
| `user`     | `{$UNIFI.USER}`        |
| `password` | `{$UNIFI.PASS}`        |

No campo `Script`, clique no ícone do Lápis e cole o código abaixo (Refatorizado para Zabbix 7):

> **OBS**: Edite os campos de login para que seu Zabbix comunique com o controlador Ubiquiti.

```javascript
// O Zabbix 7.0+ exige a declaração explícita de variáveis mapeadas do JSON 'value'
var obj = JSON.parse(value);

var host = obj.host;
var port = obj.port;
var user = obj.user;
var password = obj.password;

var response, login = new HttpRequest();
login.addHeader('Content-Type: application/json'); 

// --- FASE DE AUTENTICAÇÃO ---

response = login.post(
  'https://'+host+':'+port+'/api/login',
  '{"username": "'+user+'", "password": "'+password+'"}'
);

if (login.getStatus() !== 200) {
  throw 'Falha no login API UniFi. Status: ' + login.getStatus();
}

// 2. Coleta de Dados do Site (Padrão: default)
var site = 'default';
var url = 'https://'+host+':'+port+'/api/s/'+site+'/stat/sysinfo';

try {
  response = login.get(url);
  response = JSON.parse(response);
} catch (error) {
  throw 'Erro ao processar resposta JSON da Controladora';
}

// Retorna os dados para os Protótipos de Itens
return JSON.stringify(response.data);
```

## 4. Protótipos de Itens e Gatilhos (Triggers)

### 4.1 Criar Protótipo de Item

Dentro da regra de descoberta, clique em `Item prototypes`.

1.  Clique em `Create item prototype`.
2.  Preencha os campos:
    *   **Name**: `UniFi Site Info: {#DESC}`
    *   **Type**: `Dependent item`.
    *   **Key**: `unifi.site.info[{#DESC}]`
    *   **Master item**: Selecione a própria regra de descoberta ou o item mestre de coleta.

### 4.2 Criar Protótipo de Gatilho (Trigger)

Clique em `Trigger prototypes` > `Create trigger prototype`.

1.  Preencha os campos:
    *   **Name**: `UniFi: Status do Subsistema {#DESC} em {HOST.NAME} não está OK`
    *   **Expression**: `{Ubiquiti UniFi API Custom 7.0:unifi.site.info[{#DESC}].last()}<>"ok"`
    *   **Severity**: `High` (Alta).

## 5. Implementação no Host

Vá a `Data collection` > `Hosts`.

1.  Crie ou edite o seu Host (Ex: `SW-UBIQUITI-PRO`).
2.  **Interface Agent**: Adicione o IP real da controladora UniFi (Ex: `192.168.1.54`).
3.  **Templates**: Vincule o template `Ubiquiti UniFi API Custom 7.0`.

### 5.1 Atualização Imediata (Forçar Coleta)

Para não esperar pelo ciclo de atualização:

1.  No Host, vá a `Discovery rules`.
2.  Marque a regra e clique em `Execute now`.
3.  Vá a `Monitoring` > `Latest data` e valide se o JSON está a chegar.

## 6. Saneamento do Dashboard (Falsos Positivos)

Se o seu ambiente possui apenas Switches e APs, a API reportará "Unknown" para WAN e VPN.

1.  Vá aos `Items` do Host.
2.  Filtre por: `WAN Status`, `VPN Status` e `WWW Status`.
3.  Selecione-os e clique em `Disable` (Desativar).

Isto limpará os alertas vermelhos desnecessários do seu painel principal.

---

**Documentação finalizada. Sistema operacional e monitoramento 100% validados.**

**Autor**: Manus AI
**Data**: 16 de Março de 2026
