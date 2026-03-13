# Fluxo de Atendimento com IA 

Este repositório contém o fluxo de automação desenvolvido no [n8n](https://n8n.io/) para gerenciar um agente de Inteligência Artificial no WhatsApp. O sistema recebe mensagens, organiza filas de requisições para evitar gargalos, consulta uma IA configurada no Dify e gerencia o histórico de conversas em um banco de dados.

## 🚀 Funcionalidades Principais

* **Recepção de Webhooks:** Integração com a API do WhatsApp (Evolution API) para recebimento de mensagens em tempo real.
* **Sistema de Fila com Redis:** Previne que mensagens enviadas muito rápido pelo usuário quebrem o contexto da IA, agrupando-as usando *debounce* e gerenciamento de filas.
* **Gerenciamento de Banco de Dados:** Utiliza fluxos auxiliares (Sub-workflows) integrados ao Supabase para criar, buscar e atualizar dados de clientes e IDs de conversas.
* **Integração com IA (Dify):** Envio do contexto tratado para a engine de inteligência artificial do Dify, retornando respostas personalizadas.
* **Transbordo para Humano (Standby):** O fluxo possui uma estrutura lógica projetada para identificar a necessidade de atendimento humano e alertar um consultor. **Aviso:** Esta funcionalidade encontra-se temporariamente em *standby* enquanto o aplicativo de gerenciamento de atendentes passa por atualizações. O roteamento no n8n já está previsto, mas a ponta final não está ativa.

## 🛠️ Tecnologias e Dependências

Para rodar este fluxo perfeitamente, você precisará das seguintes ferramentas configuradas:
* **n8n** (Recomendado versão 1.0+)
* **Redis** (Para o sistema de fila de mensagens `fila_v0_`)
* **Supabase** (Ou outro banco relacional, ajustando os sub-workflows)
* **Dify** (Motor da Inteligência Artificial)
* **Evolution API** (Ou outra API de WhatsApp compatível com envio/recebimento de Webhooks)

## 📦 Como Importar

1. Baixe o arquivo deste repositório.
2. Abra o seu painel do n8n.
3. Vá em **Workflows** > **Import from File**.
4. Selecione o arquivo baixado.
5. **Atenção:** Você precisará reconfigurar as credenciais (Supabase, Redis, HTTP Requests e Tokens do Dify) de acordo com o seu ambiente local.

## 🔒 Variáveis de Ambiente e Credenciais

Certifique-se de substituir as seguintes chaves nos nós correspondentes após a importação:
* `SEU_API_KEY_AQUI`: Sua chave de API do WhatsApp.
* `SEU_TOKEN_DIFY`: Bearer Token de acesso à API do Dify.
* URLs das requisições HTTP (`https://sua-api.com/...`).

---
*Desenvolvido com foco na experiência do cliente e escalabilidade de atendimento.*
