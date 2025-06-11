# 🚗 Sistema Interno - Autoescola Ideal

Sistema completo para gestão interna da Autoescola Ideal com funcionalidades para mensagens, pagamentos, cobranças e **módulo avançado de extratos financeiros**.

## 🎯 **FUNCIONALIDADES**

### 📱 **Sistema Original**
- **Mensagens**: Envio de WhatsApp com templates personalizados
- **Pagamentos**: Registro de contas BTG (Pix/Boleto)
- **Cobranças**: Cadastro com informações de cliente e serviço
- **Login/Logout**: Sistema de autenticação por unidades

### 📊 **Módulo de Extratos (NOVO)**
- **Consulta Avançada**: Filtros por unidade, data e competência
- **Busca em Tempo Real**: Pesquisa por nome, forma de pagamento, etc.
- **Estatísticas Automáticas**: Total de registros, valores e médias
- **Integração Google Sheets**: Dados sempre atualizados
- **API REST**: Backend Node.js para performance
- **Exportação CSV**: Download dos dados filtrados

## 🚀 **INSTALAÇÃO**

### **Pré-requisitos**
- Node.js 14+ instalado
- Conta Google com acesso às planilhas
- Credenciais do Google Sheets API

### **1. Clonar/Baixar o Projeto**
```bash
# Se usando Git
git clone [url-do-projeto]
cd sistema-interno-autoescola-ideal

# Ou baixar e extrair o ZIP
```

### **2. Instalar Dependências**
```bash
npm install
```

### **3. Configurar Credenciais do Google**

#### **3.1 Criar Service Account**
1. Acesse [Google Cloud Console](https://console.cloud.google.com/)
2. Crie um novo projeto ou selecione um existente
3. Ative a API do Google Sheets
4. Vá em "Credenciais" > "Criar Credenciais" > "Conta de Serviço"
5. Baixe o arquivo JSON das credenciais

#### **3.2 Configurar Arquivo de Credenciais**
```bash
# Copiar arquivo de exemplo
cp config/credentials.json.example config/credentials.json

# Editar com suas credenciais reais
notepad config/credentials.json  # Windows
nano config/credentials.json     # Linux/Mac
```

### **4. Configurar Planilhas**
Edite o arquivo `config/planilhas.json` com os IDs das suas planilhas:

```json
{
  "planilhas": {
    "VILA_HELENA": {
      "id": "SEU_ID_DA_PLANILHA_AQUI",
      "nome": "Vila Helena",
      "range": "A:F"
    },
    "VILA_PROGRESSO": {
      "id": "SEU_OUTRO_ID_AQUI", 
      "nome": "Vila Progresso",
      "range": "A:F"
    }
  }
}
```

### **5. Compartilhar Planilhas**
Compartilhe suas planilhas Google Sheets com o email da service account (encontrado no arquivo credentials.json).

## 🏃 **EXECUTAR O SISTEMA**

### **Desenvolvimento**
```bash
npm run dev
# ou
nodemon server.js
```

### **Produção**
```bash
npm start
# ou
node server.js
```

O sistema estará disponível em:
- **Interface Web**: http://localhost:3000
- **API de Extratos**: http://localhost:3000/api/extrato
- **Health Check**: http://localhost:3000/api/health

## 🌐 **ENDPOINTS DA API**

### **📊 GET /api/extrato**
Buscar extratos com filtros

**Parâmetros:**
- `unidade` (opcional): Código da unidade (ex: VILA_HELENA)
- `dataInicio` (opcional): Data inicial (YYYY-MM-DD)
- `dataFim` (opcional): Data final (YYYY-MM-DD)
- `competencia` (opcional): Mês/ano (YYYY-MM)

**Exemplo:**
```bash
curl "http://localhost:3000/api/extrato?unidade=VILA_HELENA&dataInicio=2024-01-01"
```

### **🏢 GET /api/unidades**
Listar unidades disponíveis

### **📈 GET /api/extrato/estatisticas**
Buscar apenas estatísticas (mais rápido)

### **📄 GET /api/extrato/exportar**
Exportar dados para CSV

### **❤️ GET /api/health**
Verificar status do sistema

## 🎨 **COMO USAR**

### **1. Login**
1. Acesse http://localhost:3000
2. Faça login com uma das unidades configuradas
3. Navegue pelas abas do sistema

### **2. Módulo de Extratos**
1. Clique na aba "📊 Extrato Unidade"
2. Selecione filtros desejados:
   - **Unidade**: Escolha uma unidade específica ou "Todas"
   - **Competência**: Selecione mês/ano
   - **Período**: Defina datas específicas
3. Use a busca para encontrar registros específicos
4. Veja estatísticas em tempo real
5. Exporte dados se necessário

### **3. Outros Módulos**
- **💬 Mensagem**: Templates de WhatsApp personalizados
- **💳 Pagamento**: Registro de contas BTG
- **📋 Cobrança**: Cadastro de clientes e serviços

## 🔧 **CONFIGURAÇÕES AVANÇADAS**

### **Variáveis de Ambiente**
Crie um arquivo `.env` (opcional):
```bash
PORT=3000
NODE_ENV=production
GOOGLE_CREDENTIALS_PATH=./config/credentials.json
PLANILHAS_CONFIG_PATH=./config/planilhas.json
```

### **Personalizar Planilhas**
Para adicionar novas unidades, edite `config/planilhas.json`:
```json
{
  "planilhas": {
    "NOVA_UNIDADE": {
      "id": "ID_DA_PLANILHA",
      "nome": "Nome da Unidade", 
      "range": "A:F"
    }
  }
}
```

### **Formato das Planilhas Google Sheets**
Suas planilhas devem ter as colunas:
- **A (Nome Cliente)**: Nome do aluno
- **B (Valor)**: Valor do pagamento
- **C (Data de Pagamento)**: Data no formato DD/MM/YYYY
- **D (Forma de Pagamento)**: Pix, Boleto, etc.
- **E (Unidade)**: Nome da unidade
- **F (Observações)**: Descrições adicionais

## 🚀 **DEPLOY PARA PRODUÇÃO**

### **Opção 1: Servidor VPS**
```bash
# Instalar PM2 (gerenciador de processos)
npm install -g pm2

# Iniciar aplicação
pm2 start server.js --name "autoescola-sistema"

# Configurar inicialização automática
pm2 startup
pm2 save
```

### **Opção 2: Heroku**
```bash
# Instalar Heroku CLI
# Criar app
heroku create seu-app-name

# Configurar variáveis
heroku config:set NODE_ENV=production

# Deploy
git push heroku main
```

### **Opção 3: Netlify (Frontend + Serverless)**
- Frontend será servido estaticamente
- APIs como funções serverless

## 🔒 **SEGURANÇA**

### **Credenciais**
- ⚠️ **NUNCA** commite `credentials.json`
- Use variáveis de ambiente em produção
- Rotacione credenciais periodicamente

### **Rate Limiting**
Em produção, adicione rate limiting:
```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100 // máximo 100 requests por IP
});

app.use('/api', limiter);
```

## 🐛 **SOLUÇÃO DE PROBLEMAS**

### **Erro: "Arquivo de credenciais não encontrado"**
- Verifique se `config/credentials.json` existe
- Confirme se o caminho está correto
- Valide o formato JSON

### **Erro: "Unidade não encontrada"**
- Verifique `config/planilhas.json`
- Confirme os IDs das planilhas
- Teste acesso manual às planilhas

### **Erro: "Permission denied"**
- Compartilhe planilhas com email da service account
- Verifique permissões de leitura
- Confirme se API do Sheets está ativa

### **Sistema carrega mas sem dados**
- Verifique logs do servidor no terminal
- Teste endpoint `/api/health`
- Confirme formato das planilhas

## 📋 **LOGS E DEBUG**

```bash
# Ver logs em tempo real
tail -f logs/app.log

# Debug específico
DEBUG=extrato:* npm start

# Verificar status
curl http://localhost:3000/api/health
```

## 🤝 **SUPORTE**

### **Logs Importantes**
- ✅ API funcionando: "Módulo de extratos integrado com sucesso"
- ❌ Erro credenciais: "Arquivo de credenciais não encontrado"
- ⚠️ Planilha vazia: "Nenhum dado encontrado na planilha"

### **Checklist de Verificação**
- [ ] Node.js instalado (versão 14+)
- [ ] Dependências instaladas (`npm install`)
- [ ] Arquivo `credentials.json` configurado
- [ ] Arquivo `planilhas.json` com IDs corretos
- [ ] Planilhas compartilhadas com service account
- [ ] API do Google Sheets ativada
- [ ] Porta 3000 disponível

---

## 🎉 **PRONTO!**

Seu sistema está configurado e funcionando! 

- 🌐 **Acesse**: http://localhost:3000
- 📊 **API**: http://localhost:3000/api/extrato
- 💬 **Suporte**: Verifique logs para problemas

**Desenvolvido com ❤️ para Autoescola Ideal** 