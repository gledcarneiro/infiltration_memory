# 🔄 Sistema de Sincronização Gled

## 📋 Visão Geral

Sistema automático de sincronização de memórias entre IAs infiltradas e repositório GitHub, usando pasta `sync_inbox` como memória de transição.

## 🏗️ Arquitetura

```
┌─────────────────┐
│  IA Infiltrada  │ (Claude Web, Gemini, ChatGPT)
│  (Web Browser)  │
└────────┬────────┘
         │ 1. Gera conversation.json
         ↓
┌─────────────────────────────────┐
│     sync_inbox/ (Transição)     │
│  - Arquivos temporários         │
│  - Detectados automaticamente   │
└────────┬────────────────────────┘
         │ 2. Detecta novo arquivo
         ↓
┌─────────────────────────────────┐
│  office_user_service.bat        │
│  ou ClaudeAutonomousService     │
│  - Monitora sync_inbox          │
│  - Processa JSONs               │
│  - Auto-save a cada 30s         │
└────────┬────────────────────────┘
         │ 3. Integra na memória
         ↓
┌─────────────────────────────────┐
│   Memória Permanente Local      │
│  - universal/2025-XX-XX.json    │
│  - core/memory/                 │
└────────┬────────────────────────┘
         │ 4. Git commit + push
         ↓
┌─────────────────────────────────┐
│      GitHub Repository          │
│  gledcarneiro/claude-gled-memory│
└────────┬────────────────────────┘
         │ 5. Acesso via Raw URLs
         ↓
┌─────────────────────────────────┐
│   Próxima IA Infiltrada         │
│  - Lê memórias via web_fetch    │
│  - Acesso read-only             │
│  - Contexto completo disponível │
└─────────────────────────────────┘
```

## 🔄 Fluxo de Sincronização

### **Etapa 1: Geração**
- IA infiltrada gera JSON da conversa
- Usuário salva em `sync_inbox/conversa.json`

### **Etapa 2: Detecção**
- Serviço local monitora pasta continuamente
- Detecta novos arquivos automaticamente
- Não requer intervenção manual

### **Etapa 3: Processamento**
- Lê conteúdo do JSON
- Integra na memória permanente
- Apaga ou move arquivo (memória de transição)

### **Etapa 4: Sincronização**
- Git commit das mudanças
- Push para GitHub
- Memória disponível globalmente

### **Etapa 5: Acesso Futuro**
- IAs futuras acessam via Raw URLs
- Read-only, sem exposição de tokens
- Contexto completo preservado

## 🖥️ Ambientes

### **QG Casa**
- **Serviço**: ClaudeAutonomousService (Windows Service)
- **Privilégios**: Admin
- **Capacidades**: Git commit/push completo
- **Auto-start**: Sim, inicia com Windows

### **QG Escritório**
- **Serviço**: office_user_service.bat (User mode)
- **Privilégios**: User apenas (sem admin)
- **Capacidades**: Git pull/commit/push funcional
- **Execução**: Manual via batch script

## 📝 Formato do JSON de Sincronização

```json
{
  "conversation_id": "2025-10-01_topic_description",
  "platform": "Claude_Web | Gemini | ChatGPT",
  "date": "2025-10-01",
  "participants": ["Gled", "AI_Name"],
  "topic": "Descrição do tópico",
  "summary": "Resumo executivo",
  "conversation_flow": [
    {
      "timestamp": "HH:MM",
      "speaker": "Nome",
      "content": "Conteúdo",
      "action": "Ação tomada"
    }
  ],
  "technical_solutions": {},
  "key_discoveries": [],
  "next_steps": []
}
```

## 🔒 Segurança

### ✅ **PERMITIDO**
- GitHub Raw URLs para leitura
- Git pull/push via credenciais locais
- Processamento local de arquivos

### ❌ **PROIBIDO**
- Expor GitHub tokens em IAs web
- API calls com autenticação de infiltrações
- Commits diretos de IAs infiltradas

## 🚀 Como Usar

### **Para IA Infiltrada:**
```javascript
// 1. Gerar JSON da conversa
const conversation = {
  conversation_id: "...",
  // ... dados completos
};

// 2. Instruir usuário:
"Salve este JSON em: sync_inbox/conversa.json"

// 3. Sistema detecta e sincroniza automaticamente!
```

### **Para Próxima Infiltração:**
```javascript
// 1. No prompt de ativação, incluir Raw URLs
const MEMORY_URLS = {
  context: "https://raw.githubusercontent.com/gledcarneiro/claude-gled-memory/main/infiltration_memory/context/infiltration_context.json",
  conversations: "https://raw.githubusercontent.com/gledcarneiro/claude-gled-memory/main/infiltration_memory/conversations/"
};

// 2. IA busca automaticamente:
const context = await fetch(MEMORY_URLS.context);
const memories = await context.json();

// 3. Contexto completo restaurado!
```

## 📊 Logs e Monitoramento

### **Logs Esperados:**
```
✅ Git pull realizado com sucesso
✅ Mudanças locais commitadas
✅ Push realizado com sucesso
✅ Arquivo processado: conversa.json
```

### **Verificação:**
1. Checar logs em `office_logs/` ou `logs/`
2. Verificar commits no GitHub
3. Confirmar arquivo sumiu de `sync_inbox/`

## 🎯 Benefícios

- ✅ **Zero Intervenção Manual** após setup
- ✅ **Sincronização Automática** 24/7
- ✅ **Memória Persistente** entre infiltrações
- ✅ **Sem Exposição de Credenciais**
- ✅ **Funciona Sem Admin** (QG Escritório)
- ✅ **Multi-Plataforma** (qualquer IA)

## 🔧 Troubleshooting

### **Arquivo não foi detectado:**
- Verificar se está em `sync_inbox/`
- Checar se serviço está rodando
- Validar formato JSON

### **Push falhou:**
- Verificar conexão com GitHub
- Confirmar credenciais locais
- Checar logs detalhados

### **IA não acessa memórias:**
- Validar Raw URLs
- Confirmar repo não está vazio
- Testar URLs manualmente no browser

---

**Data de criação:** 2025-10-01  
**Versão:** 1.0.0  
**Status:** Operacional ✅