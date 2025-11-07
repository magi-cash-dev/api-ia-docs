# Documentação da API

URL Base: `http://api-ia.tutodigital.com.br`

## Autenticação

Endpoints autenticados requerem um token de autenticação válido nos headers da requisição:

```
Authorization: Bearer <seu-token>
X-Tenant-ID: <seu-tenant-uuid>

```

## Endpoints Autenticados

### Listar Chats

Recupera uma lista paginada de chats com suporte a filtros.

**Endpoint:** `GET /chats`

**Headers:**
```
Authorization: Bearer <token>
X-Tenant-ID: <seu-tenant-uuid>
Content-Type: application/json
```

**Parâmetros de Query:**
- `page` (número, opcional) - Número da página (padrão: 1)
- `limit` (número, opcional) - Itens por página (padrão: 20)
- `status` (string, opcional) - Filtrar por status: "opened" ou "closed"
- `priority` (string, opcional) - Filtrar por prioridade: "low", "medium" ou "high"
- `channel` (string, opcional) - Filtrar por canal: "whatsapp", "chat" ou "web"
- `agentId` (string, opcional) - Filtrar por UUID do agente
- `stageId` (string, opcional) - Filtrar por UUID do estágio
- `search` (string, opcional) - Buscar no conteúdo do chat

**Resposta de Sucesso (200 OK):**
```json
{
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "tenantId": "550e8400-e29b-41d4-a716-446655440001",
      "agentId": "550e8400-e29b-41d4-a716-446655440002",
      "userId": "550e8400-e29b-41d4-a716-446655440003",
      "stageId": "550e8400-e29b-41d4-a716-446655440004",
      "name": "Chat de Suporte ao Cliente",
      "channel": "web",
      "status": "opened",
      "priority": "medium",
      "remoteJid": "web-session-12345",
      "isAiEnabled": true,
      "metadata": {},
      "fupSentAt": null,
      "fupStep": 0,
      "username": "joao_silva",
      "createdAt": "2025-10-29T10:30:00.000Z",
      "updatedAt": "2025-10-29T10:35:00.000Z",
      "lastMessage": {
        "text": "Obrigado pela sua ajuda!",
        "type": "conversation",
        "createdAt": "2025-10-29T10:35:00.000Z"
      },
      "tags": [
        {
          "id": "tag-uuid-1",
          "name": "Suporte",
          "color": "#FF5733"
        }
      ],
      "stage": {
        "id": "550e8400-e29b-41d4-a716-446655440004",
        "name": "Em Andamento",
        "order": 2
      }
    }
  ],
  "meta": {
    "total": 150,
    "page": 1,
    "limit": 20,
    "totalPages": 8
  }
}
```

**Exemplo:**
```bash
curl -X GET "http://api-ia.tutodigital.com.br/chats?sortBy=createdAt&sortOrder=desc" \ -H "Authorization: Bearer <token>" -H "X-Tenant-ID: <seu-tenant-uuid>"

---

### Obter Chat por ID

Recupera informações detalhadas sobre um chat específico.

**Endpoint:** `GET /chats/{id}`

**Headers:**
```
Authorization: Bearer <token>
```

**Parâmetros de Rota:**
- `id` (string, obrigatório) - UUID do chat

**Resposta de Sucesso (200 OK):**
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "tenantId": "550e8400-e29b-41d4-a716-446655440001",
  "agentId": "550e8400-e29b-41d4-a716-446655440002",
  "userId": "550e8400-e29b-41d4-a716-446655440003",
  "stageId": "550e8400-e29b-41d4-a716-446655440004",
  "name": "Chat de Suporte ao Cliente",
  "channel": "web",
  "status": "opened",
  "priority": "medium",
  "remoteJid": "web-session-12345",
  "isAiEnabled": true,
  "metadata": {
    "source": "website",
    "page": "/contato"
  },
  "fupSentAt": null,
  "fupStep": 0,
  "username": "joao_silva",
  "createdAt": "2025-10-29T10:30:00.000Z",
  "updatedAt": "2025-10-29T10:35:00.000Z",
  "lastMessage": {
    "text": "Obrigado pela sua ajuda!",
    "type": "conversation",
    "createdAt": "2025-10-29T10:35:00.000Z"
  },
  "tags": [],
  "stage": {
    "id": "550e8400-e29b-41d4-a716-446655440004",
    "name": "Em Andamento",
    "order": 2
  }
}
```

**Respostas de Erro:**
```json
// 404 Not Found
{
  "error": "Chat not found"
}
```

**Exemplo:**
```bash
curl -X GET http://api-ia.tutodigital.com.br/chats/550e8400-e29b-41d4-a716-446655440000 \ -H "Authorization: Bearer <token>"  -H "X-Tenant-ID: <seu-tenant-uuid>"
```

---

### Atualizar Chat

Atualiza propriedades do chat como status, prioridade, configurações de IA ou metadata.

**Endpoint:** `PUT /chats/{id}`

**Headers:**
```
Authorization: Bearer <token>
X-Tenant-ID: <seu-tenant-uuid>
Content-Type: application/json
```

**Parâmetros de Rota:**
- `id` (string, obrigatório) - UUID do chat

**Corpo da Requisição:**
```json
{
  "status": "opened | closed (opcional)",
  "priority": "low | medium | high (opcional)",
  "isAiEnabled": "boolean (opcional)",
  "stageId": "string uuid (opcional)",
  "metadata": "object (opcional)",
  "username": "string (opcional)"
}
```

**Exemplo de Corpo da Requisição:**
```json
{
  "status": "closed",
  "priority": "low",
  "isAiEnabled": false,
  "metadata": {
    "resolutionNotes": "Problema resolvido com sucesso"
  }
}
```

**Resposta de Sucesso (200 OK):**
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "status": "closed",
  "priority": "low",
  "isAiEnabled": false,
  "metadata": {
    "resolutionNotes": "Problema resolvido com sucesso"
  },
  "updatedAt": "2025-10-29T11:00:00.000Z"
}
```

**Respostas de Erro:**
```json
// 404 Not Found
{
  "error": "REGISTER_NOT_FOUND"
}
```

**Exemplo:**
```bash
curl -X PUT http://api-ia.tutodigital.com.br/chats/550e8400-e29b-41d4-a716-446655440000 \
  -H "Authorization: Bearer <token>" \
  -H "X-Tenant-ID: <seu-tenant-uuid>"
  -H "Content-Type: application/json" \
  -d '{
    "status": "closed",
    "priority": "low"
  }'
```

---

### Deletar Chat

Deleta permanentemente um chat e todas as suas mensagens associadas.

**Endpoint:** `DELETE /chats/{id}`

**Headers:**
```
Authorization: Bearer <token>
X-Tenant-ID: <seu-tenant-uuid>
```

**Parâmetros de Rota:**
- `id` (string, obrigatório) - UUID do chat

**Resposta de Sucesso (204 No Content):**
```
(Corpo de resposta vazio)
```

**Respostas de Erro:**
```json
// 404 Not Found
{
  "error": "Chat not found"
}
```

**Exemplo:**
```bash
curl -X DELETE http://api-ia.tutodigital.com.br/chats/550e8400-e29b-41d4-a716-446655440000 \
  -H "Authorization: Bearer <token>"
  -H "X-Tenant-ID: <seu-tenant-uuid>"
```

---

### Listar Mensagens do Chat

Recupera todas as mensagens de um chat autenticado específico.

**Endpoint:** `GET /chats/{id}/messages`

**Headers:**
```
Authorization: Bearer <token>
X-Tenant-ID: <seu-tenant-uuid>
```

**Parâmetros de Rota:**
- `id` (string, obrigatório) - UUID do chat

**Parâmetros de Query:**
- `limit` (número, opcional) - Número máximo de mensagens a retornar
- `offset` (número, opcional) - Número de mensagens a pular

**Resposta de Sucesso (200 OK):**
```json
[
  {
    "id": "650e8400-e29b-41d4-a716-446655440000",
    "tenantId": "550e8400-e29b-41d4-a716-446655440001",
    "chatId": "550e8400-e29b-41d4-a716-446655440000",
    "userId": "550e8400-e29b-41d4-a716-446655440003",
    "agentId": null,
    "type": "conversation",
    "status": "read",
    "key": {
      "remoteJid": "web-session-12345",
      "fromMe": false,
      "id": "msg-123"
    },
    "message": {
      "conversation": "Olá, preciso de ajuda"
    },
    "contextInfo": null,
    "sourceCreatedAt": "2025-10-29T10:30:00.000Z",
    "sourceKeyId": "msg-123",
    "remoteJid": "web-session-12345",
    "createdAt": "2025-10-29T10:30:00.000Z",
    "updatedAt": "2025-10-29T10:30:00.000Z"
  }
]
```

**Exemplo:**
```bash
curl -X GET "http://api-ia.tutodigital.com.br/chats/550e8400-e29b-41d4-a716-446655440000/messages?limit=50" \
  -H "Authorization: Bearer <token>"
  -H "X-Tenant-ID: <seu-tenant-uuid>"
```

---

### Enviar Mensagem no Chat

Envia uma nova mensagem em um contexto de chat autenticado.

**Endpoint:** `POST /chats/{id}/messages`

**Headers:**
```
Authorization: Bearer <token>
Content-Type: application/json
X-Tenant-ID: <seu-tenant-uuid>
```

**Parâmetros de Rota:**
- `id` (string, obrigatório) - UUID do chat

**Corpo da Requisição:**
```json
{
  "message": "string (obrigatório)"
}
```

**Resposta de Sucesso (200 OK):**
```json
{
  "id": "650e8400-e29b-41d4-a716-446655440000",
  "chatId": "550e8400-e29b-41d4-a716-446655440000",
  "message": {
    "conversation": "Seu texto de mensagem"
  },
  "createdAt": "2025-10-29T10:30:00.000Z"
}
```

**Exemplo:**
```bash
curl -X POST http://api-ia.tutodigital.com.br/chats/550e8400-e29b-41d4-a716-446655440000/messages \
  -H "Authorization: Bearer <token>" \
  -H "X-Tenant-ID: <seu-tenant-uuid>" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Esta é uma mensagem de teste"
  }'
```

---

## Modelos de Dados

### Objeto Chat

```typescript
{
  id: string                    // UUID
  tenantId: string              // UUID - Identificador da organização
  agentId: string               // UUID - Agente de IA gerenciando o chat
  userId: string                // UUID - Usuário associado ao chat
  stageId?: string              // UUID - Estágio atual no fluxo de trabalho
  name: string                  // Nome de exibição do chat
  channel: "whatsapp" | "chat" | "web"  // Canal de comunicação
  status: "opened" | "closed"   // Status do chat
  priority: "low" | "medium" | "high"   // Nível de prioridade
  remoteJid: string             // Identificador único da sessão
  isAiEnabled: boolean          // Se respostas de IA estão habilitadas
  metadata: object              // Pares chave-valor de metadata customizada
  fupSentAt?: Date              // Timestamp de envio de follow-up
  fupStep: number               // Contador de etapas de follow-up
  username?: string             // Nome de usuário de exibição
  createdAt: Date               // Timestamp de criação
  updatedAt: Date               // Timestamp da última atualização
  lastMessage?: {               // Prévia da última mensagem
    text: string | null
    type: string | null
    createdAt: Date | null
  }
  tags?: Tag[]                  // Tags associadas
  stage?: Stage                 // Detalhes do estágio atual
}
```

### Objeto Message

```typescript
{
  id: string                    // UUID
  tenantId: string              // UUID - Identificador da organização
  chatId: string                // UUID - Chat associado
  userId?: string               // UUID - Usuário que enviou (se mensagem de usuário)
  agentId?: string              // UUID - Agente que enviou (se mensagem de agente)
  type: string                  // Tipo de mensagem (ex: "conversation", "image")
  status: "pending" | "read" | "delivery"  // Status de entrega
  key: {
    remoteJid: string           // Identificador da sessão
    fromMe: boolean             // false = usuário, true = agente/IA
    id: string                  // ID único da mensagem
  }
  message: {
    conversation?: string       // Conteúdo de texto (para mensagens de texto)
    // Outros campos de tipo de mensagem...
  }
  contextInfo?: {               // Contexto de citação/resposta
    stanzaId: string
    participant: string
    quotedMessage: {
      conversation: string
    }
  }
  sourceCreatedAt: Date         // Timestamp de criação original
  sourceKeyId: string           // ID da mensagem no sistema fonte
  remoteJid: string             // Identificador da sessão
  createdAt: Date               // Timestamp de criação no banco de dados
  updatedAt: Date               // Timestamp da última atualização
}
```
```

---

### Cenários Comuns de Erro

**500 Internal Server Error**
- Erros de banco de dados
- Agente não encontrado
- Erros inesperados do servidor

---
