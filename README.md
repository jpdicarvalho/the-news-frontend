# 📢 The News - Plataforma Web Funcional

Bem-vindo ao projeto de gameficação do **The News**, um sistema para monitoramento de engajamento em newsletters, incluindo streaks de usuários, estatísticas gerais e filtros dinâmicos no dashboard administrativo.

## 🚀 Tecnologias Utilizadas

### **🛠️ Stacks**
- **Backend**: [Hono.js](https://hono.dev/) (framework minimalista para Cloudflare Workers)
- **Banco de Dados**: Cloudflare D1 (SQLite compatível com Workers)
- **Autenticação**: JWT (JSON Web Token)
- **Frontend**: React.js + Axios para requisições
- **Deploy**: Cloudflare Workers
- **Testes**: Postman, Insomnia e logs no Cloudflare Wrangler

### **⚠️ Desafios Enfrentados**
1. **Webhook da empresa não funcionando** → Criei um simulador de webhook no Cloudflare Workers.
2. **Banco D1 sem suporte a algumas funções SQL** → Adaptei queries para compatibilidade.
3. **CORS bloqueando requisições** → Implementei middleware para permitir requests do frontend.
4. **JWT Storage** → Implementei persistência do token com validação na API.

### **👤 Organização do Código**
Adotei **modularização**:
- **`index.ts`** → Arquivo principal, onde as rotas estão contidas.
- **`middleware/*.ts`** → Middleware de autenticação JWT.

---

## 📊 Estrutura dos Dados

### **🐄 Estrutura SQL**

```sql
CREATE TABLE users (
    id TEXT PRIMARY KEY,
    email TEXT UNIQUE NOT NULL,
    streak INTEGER DEFAULT 0,
    last_opened TEXT
);

CREATE TABLE newsletters (
    id TEXT PRIMARY KEY,
    user_id TEXT REFERENCES users(id),
    opened_at TEXT
);

CREATE TABLE sessions (
    user_id TEXT PRIMARY KEY REFERENCES users(id),
    token TEXT NOT NULL,
    expires_at TEXT NOT NULL
);
```

### **📥 Inserções e Consultas**
- **Webhook** → Insere leituras automaticamente ao ser acionado.
- **Login** → Recupera usuário e gera JWT.
- **Dashboard** → Filtros dinâmicos via query params.

### **📊 Escalabilidade**
O D1 é limitado em **escrita concorrente**, mas eficiente para leitura. Se precisar escalar:
- Usar **Redis** para cache.
- Migrar para **PostgreSQL** ou **PlanetScale (MySQL)**.
- Implementar **fila de processamento** para registros massivos.

---

## ✅ Testes Realizados

### **🔬 Tipos de Testes**
- **API Testes**: Testei todas as rotas via Postman.
- **Webhook Teste**: Criei um simulador enviando requisições a cada 5 minutos.
- **Banco de Dados**: Inserções de dados de teste retroativos de 30 dias.
- **Autenticação JWT**: Testei expiração de token e middleware de segurança.
- **Dashboard**: Filtros aplicados corretamente e dados formatados para gráficos.

### **⏳ Tempo de Desenvolvimento**
- **Backend**: 2 dias
- **Webhook e Testes**: 1 dia
- **Frontend (dashboard + integração)**: 1 dia
- **Refinamento e correções**: 4 horas
- **Total**: **~4 dias e 4 horas**

---

## 📌 Como Rodar o Projeto

### **🌐 Backend**
🚀 O backend já está em produção na **Cloudflare Workers**, então **não é necessário rodá-lo localmente**.

🔗 [Repositório Backend](https://github.com/jpdicarvalho/the-news-backend)

🔗 [Repositório Webhook Simulator](https://github.com/jpdicarvalho/the-news-webhook-simulator)


### **🖥️ Frontend**
1. Clone o repositório do frontend (React + Vite):
   ```bash
   git clone https://github.com/seu-repo/the-news-frontend.git
2. Acesse o diretório
   ```bash
   cd the-news-frontend
3. Instale as dependências
   ```bash
   npm install
4. Execulte o projeto
   ```bash
   npm run dev
---
### **Para acessar como usuário convencional use:**
     teste@teste.com
 
### **Para acessar como usuário admin use:**
    admin@admin.com	
---
        
## Contato
Caso tenha dúvidas ou sugestões:
- 💎 LinkedIn: [Meu LinkedIn](https://www.linkedin.com/in/jpdicarvalho/)
