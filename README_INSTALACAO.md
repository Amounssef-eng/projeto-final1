# 📦 Gestor de Produtos - Aplicação Desktop

Aplicação de gestão de produtos com interface moderna, desenvolvida em Electron + Node.js + MySQL.

## 🚀 Funcionalidades

✅ Interface moderna e responsiva
✅ Adicionar produtos
✅ Visualizar lista de produtos
✅ Remover produtos acidentalmente adicionados
✅ Sincronização com MySQL Workbench
✅ Aplicação Desktop (Windows, Mac, Linux)

## 📋 Pré-requisitos

- **Node.js** (v14 ou superior) - [Download](https://nodejs.org/)
- **MySQL Server** - [Download](https://dev.mysql.com/downloads/mysql/)
- **MySQL Workbench** (opcional, para gerenciar BD) - [Download](https://dev.mysql.com/downloads/workbench/)

## 🔧 Instalação

### 1. Clone ou Extraia o Projeto
```bash
# Se estiver em um ZIP:
# Extraia o arquivo e abra a pasta no terminal

# Se estiver no GitHub:
git clone https://github.com/seu-usuario/gestor-de-produtos.git
cd gestor-de-produtos
```

### 2. Instale as Dependências
```bash
# Dependências do projeto principal
npm install

# Dependências do backend
cd backend
npm install
cd ..
```

### 3. Configure o MySQL

Abra o **MySQL Workbench** e execute:

```sql
-- Criar banco de dados
CREATE DATABASE sistema_gestao;

-- Usar o banco
USE sistema_gestao;

-- Criar tabela
CREATE TABLE categoria (
  id_categoria INT PRIMARY KEY AUTO_INCREMENT,
  nome VARCHAR(100) NOT NULL
);

CREATE TABLE produto (
  id_produto INT PRIMARY KEY AUTO_INCREMENT,
  nome VARCHAR(100) NOT NULL,
  preco DECIMAL(10,2) NOT NULL,
  quantidade INT NOT NULL,
  id_categoria INT,
  FOREIGN KEY (id_categoria) REFERENCES categoria(id_categoria)
);

-- Inserir categorias de exemplo
INSERT INTO categoria (nome) VALUES ('Eletrónicos'), ('Informática'), ('Escritório');

-- Inserir produtos de exemplo
INSERT INTO produto (nome, preco, quantidade, id_categoria) VALUES
('Teclado Mecânico', 59.90, 20, 2),
('Mouse Gamer', 39.90, 15, 2),
('Monitor 24 Polegadas', 149.99, 10, 1);
```

### 4. Execute a Aplicação

**Opção 1 - Modo Desenvolvimento (servidor + app automático):**
```bash
npm run dev
```

**Opção 2 - Apenas a Aplicação (servidor já rodando):**
```bash
npm start
```

## 📦 Compilar para Distribuição

Para criar um executável Windows:

```bash
npm run build
```

O arquivo será gerado em: `dist/win-unpacked/Gestor de Produtos.exe`

## 📁 Estrutura do Projeto

```
gestor-de-produtos/
├── main.js                 # Arquivo principal Electron
├── preload.js              # Script de segurança
├── index.html              # Interface (HTML/CSS/JS)
├── package.json            # Dependências
├── backend/
│   ├── server.js           # API Express
│   └── package.json        # Dependências backend
└── dist/                   # Executável compilado (após npm run build)
```

## 🔌 Configuração do MySQL

Se sua conexão MySQL for diferente, edite `backend/server.js`:

```javascript
const db = mysql.createConnection({
  host: "localhost",      // Seu host
  user: "root",           // Seu usuário
  password: "sua_senha",  // Sua senha
  database: "sistema_gestao"
});
```

## 🐛 Troubleshooting

### "Não consigo conectar ao MySQL"
- Verifique se o MySQL Server está rodando
- Confirme as credenciais (usuário e senha)
- Verifique se o banco `sistema_gestao` foi criado

### "Porta 3001 já está em uso"
```bash
# Windows
netstat -ano | findstr ":3001"
taskkill /PID <PID> /F
```

### "ffmpeg.dll não encontrado" (ao compilar)
Isto é normal e não afeta a funcionalidade. O Electron inclui automaticamente.

## 📧 Suporte

Para dúvidas ou problemas:
1. Abra o console (Ctrl+Shift+I) na aplicação
2. Verifique se o backend está rodando na porta 3001
3. Confirme a conexão MySQL

## 📄 Licença

ISC

## 🎯 Próximos Passos

- [ ] Adicionar autenticação de usuários
- [ ] Implementar categorias de produtos
- [ ] Adicionar relatórios
- [ ] Sincronização online

---

**Desenvolvido com ❤️ em Node.js e Electron**
