

## Documentação Encontrada — Arquitetura-Alvo para Integrações SharePoint Online via Gateway

### 1. Service Principal no Entra ID com Autenticação App-Only (não-interativa)
**Artigo:** *Granting access via Entra ID Application Permissions*
**URL:** `https://learn.microsoft.com/en-us/sharepoint/dev/solution-guidance/security-apponly-azuread`

Este artigo documenta exatamente o padrão recomendado: configurar um **App Registration no Entra ID** com permissões de aplicação (não delegadas), usando **certificado** para autenticação App-Only no SharePoint Online. O próprio artigo afirma que outros meios além de certificado são bloqueados pelo SharePoint Online para acesso App-Only. Cobre também o uso via PnP PowerShell com autenticação não-interativa.

---

### 2. Fluxo OAuth2 Client Credentials (com certificado)
**Artigo:** *OAuth 2.0 client credentials flow on the Microsoft identity platform*
**URL:** `https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-client-creds-grant-flow`

Documenta o fluxo de credenciais de cliente OAuth 2.0, incluindo os dois casos relevantes para a recomendação:
- **Caso 1:** Token com segredo compartilhado (client_secret) — não recomendado para produção
- **Caso 2:** Token com **certificado** (`client_assertion_type: urn:ietf:params:oauth:client-assertion-type:jwt-bearer`) — este é o padrão recomendado da sua arquitetura-alvo

O artigo descreve o fluxo para **daemons e contas de serviço**, exatamente o cenário de integração máquina-a-máquina (M2M) sem usuário interativo.

---

### 3. Gateway de Dados On-Premises
**Artigo:** *What is an on-premises data gateway?*
**URL:** `https://learn.microsoft.com/en-us/data-integration/gateway/service-gateway-onprem`

Documenta o gateway como ponte entre serviços cloud (Power BI, Power Automate, Logic Apps) e fontes de dados, incluindo SharePoint Online. Cobre os tipos de gateway (Enterprise/Personal), configuração e considerações operacionais.

---

### Resumo de Cobertura da Recomendação

| Elemento da Recomendação | Documentação Correspondente |
|---|---|
| Service Principal no Entra ID | `security-apponly-azuread` |
| Autenticação não-interativa com certificado | `security-apponly-azuread` + `v2-oauth2-client-creds-grant-flow` |
| Fluxo OAuth2 Client Credentials | `v2-oauth2-client-creds-grant-flow` |
| Permissões mínimas (Sites.Read.All, etc.) | `security-apponly-azuread` (exemplos com `-SharePointApplicationPermissions`) |
| Gateway corporativo | `service-gateway-onprem` |
| Eliminação de usuário/senha para contas de serviço | `security-apponly-azuread` (seção FAQ afirma que apenas certificados são aceitos) |

Os artigos sobre **auditoria/monitoramento pelo Entra ID Logs** e **separação por ambientes (DEV/UAT/PROD)** não possuem uma página única dedicada, mas são cobertos indiretamente pelas boas práticas de governança no Entra ID. Caso queira, posso buscar também a documentação específica sobre logs de auditoria do Entra ID e SharePoint, ou sobre gerenciamento de App Registrations por ambiente.