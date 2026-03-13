# Fluxo de Atendimento com IA

## 📌 Visão Geral
Este projeto consiste em um workflow desenvolvido no n8n que implementa um assistente de atendimento automatizado via WhatsApp utilizando Inteligência Artificial.

O fluxo é responsável por:
* Receber e processar mensagens de clientes.
* Consultar e gerenciar dados de clientes no banco de dados.
* Enviar o contexto para um motor de IA e retornar respostas automaticamente.
* Gerenciar o histórico de conversas.
* Preparar a estrutura para transferência para atendimento humano.

O sistema utiliza diferentes serviços integrados para compor a arquitetura de atendimento automatizado.

---

## 🧠 Arquitetura do Sistema
O fluxo integra os seguintes componentes:

| Serviço | Função |
| :--- | :--- |
| **n8n** | Orquestração da automação |
| **WhatsApp API** | Recebimento e envio de mensagens |
| **Dify AI** | Motor de inteligência artificial |
| **Supabase** | Banco de dados de clientes e conversas |
| **Redis** | Controle de fila e agrupamento de mensagens |

---

## ⚙️ Funcionamento do Fluxo

### 1️⃣ Recebimento da Mensagem
O fluxo inicia através de um Webhook, que recebe requisições HTTP enviadas pela integração com o WhatsApp. Essas requisições contêm dados como telefone e nome do cliente, conteúdo da mensagem e timestamp.

### 2️⃣ Processamento da Mensagem
Após o recebimento, um workflow auxiliar é executado para extrair o conteúdo da mensagem, identificar o telefone e o nome do remetente, além de tratar possíveis mídias (como imagens e áudios).

### 3️⃣ Validação do Cliente
O fluxo valida se o telefone do cliente foi identificado corretamente. Caso o telefone não esteja presente, o processamento é interrompido.

### 4️⃣ Consulta no Banco de Dados
O sistema consulta o banco de dados para verificar se o cliente já existe. Caso não exista, um novo registro é criado contendo o telefone, nome, identificador da conversa e o status do atendimento.

### 5️⃣ Sistema de Fila de Mensagens
Para evitar que múltiplas mensagens curtas enviadas em sequência sejam processadas separadamente, o sistema utiliza o Redis para criar uma fila de mensagens por cliente. Esse mecanismo permite agrupar o texto, aguardar um pequeno intervalo de tempo e consolidar tudo em uma única pergunta, melhorando significativamente a qualidade do contexto passado para a IA.

### 6️⃣ Envio da Mensagem para a IA
Após a consolidação, o fluxo envia a pergunta ao motor de inteligência artificial (Dify). São enviados a mensagem do cliente, o telefone, o identificador de conversa e a chave de autenticação. O motor de IA processa e retorna a resposta gerada, o ID da conversa e o contexto da interação.

### 7️⃣ Atualização do Histórico de Conversa
O ID da conversa retornado pela IA é armazenado no banco de dados para manter o contexto das próximas interações. Isso permite que a IA mantenha a continuidade lógica do atendimento.

### 8️⃣ Envio da Resposta ao Cliente
A resposta gerada é enviada de volta ao cliente através da API do WhatsApp. O sistema realiza um tratamento de texto prévio para evitar problemas com quebras de linha e caracteres especiais.

---

## 👨‍💼 Atendimento Humano
O fluxo contém uma estrutura preparada para realizar a transferência do atendimento para um consultor humano. Esse processo envolve:
* Identificação de mensagens específicas que exigem intervenção humana.
* Atualização do status do cliente no banco de dados.
* Envio de notificação ao consultor e de uma mensagem ao cliente informando a transferência.

> **⚠️ Importante:** A funcionalidade de atendimento humano está atualmente em **standby** no projeto. Devido a isso, essa parte do fluxo não foi completamente testada e o comportamento não é garantido como 100% funcional. Ajustes e validações adicionais ainda serão necessários. Portanto, essa funcionalidade deve ser considerada experimental no estado atual.

---

## ✅ Status do Projeto

| Componente | Status |
| :--- | :--- |
| Recebimento de mensagens | ✅ Funcional |
| Processamento de mensagens | ✅ Funcional |
| Integração com IA | ✅ Funcional |
| Persistência em banco de dados | ✅ Funcional |
| Sistema de fila de mensagens | ✅ Funcional |
| Envio de resposta ao cliente | ✅ Funcional |
| Transferência para humano | ⚠️ Em standby |

---

## 🚀 Possíveis Melhorias Futuras
* Finalização e homologação do fluxo de atendimento humano.
* Melhorias na detecção automática de intenção do usuário.
* Suporte ampliado para interpretação de mensagens de mídia.
* Implementação de métricas e monitoramento de atendimentos.
* Criação de um painel administrativo para acompanhamento das conversas.
* Estruturação de logs para auditoria do atendimento.

---

## 📄 Observações
Este fluxo foi projetado para ser modular, utilizando workflows auxiliares dentro do n8n para separar responsabilidades (como leitura de mídias, gerenciamento do banco de dados e comunicação com o motor de IA). Essa arquitetura facilita a manutenção, a escalabilidade e a reutilização de componentes no futuro.
