# 📢 The News API - Dashboard de Engajamento

Bem-vindo ao **The News API**, um sistema para monitoramento de engajamento em newsletters, incluindo streaks de usuários, estatísticas gerais e filtros dinâmicos no dashboard administrativo.

## 🚀 Tecnologias Utilizadas

### **🛠️ Stacks**
- **Backend**: [Hono.js](https://hono.dev/) (framework minimalista para Cloudflare Workers)
- **Banco de Dados**: Cloudflare D1 (SQLite compatível com Workers)
- **Autenticação**: JWT (JSON Web Token)
- **Frontend**: React.js + Axios para requisições
- **Deploy**: Cloudflare Workers
- **Testes**: Postman, Insomnia e logs no Cloudflare Wrangler

### **⚠️ Desafios Enfrentados**
1. **Webhook da empresa não funcionando** → Criamos um simulador de webhook no Cloudflare Workers.
2. **Banco D1 sem suporte a algumas funções SQL** → Adaptamos queries para compatibilidade.
3. **CORS bloqueando requisições** → Implementamos middleware para permitir requests do frontend.
4. **JWT Storage** → Implementamos persistência do token com validação na API.

### **📂 Organização do Código**
Adotamos **modularização**:
- **`index.ts`** → Ponto de entrada, onde rotas são importadas.
- **`routes/*.ts`** → Divisão entre `auth.routes.ts`, `user.routes.ts`, `admin.routes.ts`, etc.
- **`middleware/*.ts`** → Middleware de autenticação JWT.
- **`controllers/*.ts`** → Lógica separada das rotas para manter Clean Code.

---

## 📊 Estrutura dos Dados

### **🗄️ Estrutura SQL**

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

### **📈 Escalabilidade**
O D1 é limitado em **escrita concorrente**, mas eficiente para leitura. Se precisar escalar:
- Usar **Redis** para cache.
- Migrar para **PostgreSQL** ou **PlanetScale (MySQL)**.
- Implementar **fila de processamento** para registros massivos.

---

## ✅ Testes Realizados

### **🔬 Tipos de Testes**
- **API Testes**: Testamos todas as rotas via Postman.
- **Webhook Teste**: Criamos um simulador enviando requisições a cada 5 minutos.
- **Banco de Dados**: Inserimos dados de teste retroativos de 30 dias.
- **Autenticação JWT**: Testamos expiração de token e middleware de segurança.
- **Dashboard**: Filtros aplicados corretamente e dados formatados para gráficos.

### **⏳ Tempo de Desenvolvimento**
- **Backend**: 12 horas
- **Webhook e Testes**: 6 horas
- **Frontend (dashboard + integração)**: 8 horas
- **Refinamento e correções**: 4 horas
- **Total**: **~30 horas**

---

## 📌 Como Rodar o Projeto

### **🌐 Backend**
1. Clone o repositório:
   ```bash
   git clone https://github.com/seu-repo/the-news-api.git
   ```
2. Instale dependências:
   ```bash
   cd the-news-api && npm install
   ```
3. Configure o Cloudflare Wrangler:
   ```bash
   npx wrangler login
   ```
4. Rode localmente:
   ```bash
   npm run dev
   ```

### **🖥️ Frontend**
1. Clone o repositório do frontend.
2. Instale dependências e rode:
   ```bash
   npm install && npm start
   ```
3. Acesse `http://localhost:3000` no navegador.

---

## 📬 Contato
Caso tenha dúvidas ou sugestões:
- 📧 Email: `seuemail@email.com`
- 💼 LinkedIn: [linkedin.com/in/seuperfil](https://linkedin.com/in/seuperfil)
