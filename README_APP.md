# 📦 Gestor de Produtos - Aplicativo Desktop

Aplicação desktop para gerenciar produtos com interface moderna.

## ✅ Pré-requisitos

- Node.js (v14 ou superior)
- npm

## 🚀 Instalação

1. **Instale as dependências do projeto:**
```bash
npm install
```

2. **Instale as dependências do backend:**
```bash
cd backend
npm install
cd ..
```

## 🎮 Como Executar

### Opção 1: Modo Desenvolvimento (com servidor automático)
```bash
npm run dev
```
Isso iniciará o servidor backend e abrirá o aplicativo Electron automaticamente.

### Opção 2: Apenas o Aplicativo
Primeiro, inicie o servidor em um terminal:
```bash
cd backend
npm start
```

Depois, em outro terminal:
```bash
npm start
```

## 📦 Compilar para Distribuição

Para criar o instalador do aplicativo:

```bash
npm run build
```

Isso criará executáveis em:
- `dist/Gestor de Produtos Setup.exe` (instalador)
- `dist/Gestor de Produtos.exe` (versão portável)

## 📋 Funcionalidades

✅ Adicionar produtos
✅ Visualizar lista de produtos
✅ Remover produtos
✅ Interface responsiva e moderna
✅ Sincronização em tempo real com banco de dados

## 🛠️ Tecnologias

- **Frontend:** HTML5, CSS3, JavaScript
- **Backend:** Node.js + Express + MySQL
- **Desktop:** Electron
- **Build:** Electron Builder

## 📝 Estrutura do Projeto

```
├── main.js              # Arquivo principal do Electron
├── preload.js           # Preload script para segurança
├── index.html           # Interface do aplicativo
├── package.json         # Dependências do projeto
├── backend/
│   ├── server.js        # Servidor Express
│   └── package.json     # Dependências do backend
└── assets/              # Ícones e imagens (criar conforme necessário)
```

## 🐛 Solução de Problemas

**"Cannot find module 'electron-is-dev'"**
```bash
npm install --save-dev electron-is-dev
```

**Porta 3001 já em uso**
```bash
# Windows
netstat -ano | findstr :3001
taskkill /PID <PID> /F
```

## 📧 Suporte

Para dúvidas ou problemas, verifique o console do Electron (Ctrl+Shift+I).
