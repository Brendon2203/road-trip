# 🚀 Road Trip - Sistema de Gerenciamento de Participantes

## 📋 Descrição
Sistema web para gerenciar participantes de uma viagem, incluindo upload de comprovantes de pagamento, controle de pagamentos e sistema administrativo completo.

## ✨ Funcionalidades Principais

### 👥 Para Participantes
- **Upload de Comprovantes**: Envio seguro de imagens e PDFs
- **Visualização**: Visualizar arquivos enviados com ícone de olho
- **Autenticação por CPF**: Acesso seguro aos próprios documentos
- **Status de Pagamento**: Acompanhar pagamentos por mês

### 🔧 Para Administradores
- **Painel Completo**: Gerenciar participantes e arquivos
- **Visualização de Arquivos**: Ver todos os comprovantes enviados
- **Sistema de Filtros**: Filtrar por participante, mês e tipo de arquivo
- **Download de Arquivos**: Baixar comprovantes individualmente
- **Sistema de Backup**: Exportar/importar dados e backup automático

## 🛡️ Segurança e Armazenamento

### Firebase Storage
- **Armazenamento Seguro**: Todos os arquivos são salvos no Firebase Storage da Google
- **URLs Únicas**: Cada arquivo recebe uma URL única e segura
- **Metadados**: Informações detalhadas sobre cada upload (data, tamanho, tipo)

### Sistema de Backup
- **Backup Automático**: Criado a cada 24 horas no Firestore
- **Exportação Manual**: Exportar dados em formato JSON
- **Importação**: Restaurar dados de backup quando necessário
- **Múltiplas Camadas**: Dados protegidos em servidores da Google

## 🚀 Como Hospedar em Produção

### 1. Configuração do Firebase
```bash
# 1. Acesse o Firebase Console
# 2. Configure as regras de segurança do Storage:
```

**Regras do Storage (firebase-storage.rules):**
```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /comprovantes/{fileName} {
      allow read: if request.auth != null || 
        resource.metadata.participantId in get(/databases/$(firestore.default)/documents/viagem/data/participants/{participantId}).data.authorizedUsers;
      allow write: if request.auth != null;
    }
  }
}
```

**Regras do Firestore (firestore.rules):**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /viagem/{document=**} {
      allow read, write: if true; // Para desenvolvimento
      // Para produção, implementar autenticação adequada
    }
    match /backups/{document} {
      allow read, write: if request.auth != null;
    }
  }
}
```

### 2. Hospedagem
```bash
# Opção 1: Firebase Hosting (Recomendado)
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy

# Opção 2: Netlify
# Faça upload do arquivo index.html para o Netlify

# Opção 3: Vercel
# Conecte seu repositório GitHub ao Vercel
```

### 3. Configurações de Produção

#### Variáveis de Ambiente (se necessário):
```javascript
// Configurações do Firebase já estão no código
const firebaseConfig = {
    apiKey: "sua-api-key",
    authDomain: "seu-projeto.firebaseapp.com",
    projectId: "seu-projeto",
    storageBucket: "seu-projeto.firebasestorage.app",
    messagingSenderId: "seu-sender-id",
    appId: "seu-app-id"
};
```

#### Configurações de Segurança:
1. **Limite de Upload**: 10MB por arquivo
2. **Tipos Permitidos**: Imagens (jpg, png, gif) e PDFs
3. **Autenticação**: CPF dos participantes
4. **Backup**: Automático a cada 24 horas

## 📱 Responsividade
- ✅ Desktop (1920px+)
- ✅ Tablet (768px - 1024px)
- ✅ Mobile (320px - 767px)

## 🔧 Manutenção

### Backup Manual
```javascript
// No console do navegador (modo admin):
exportData(); // Exporta dados atuais
```

### Restaurar Backup
```javascript
// No console do navegador (modo admin):
restoreBackup(); // Restaura último backup automático
importData(); // Importa arquivo JSON manual
```

### Monitoramento
- Verificar logs do Firebase Console
- Monitorar uso do Storage
- Acompanhar backups automáticos

## 🚨 Importante para Produção

### ✅ Checklist de Segurança
- [ ] Configurar regras de segurança do Firebase
- [ ] Implementar autenticação adequada
- [ ] Configurar domínio personalizado
- [ ] Ativar HTTPS
- [ ] Configurar backup automático
- [ ] Testar upload/download de arquivos
- [ ] Verificar responsividade

### 📊 Monitoramento
- **Firebase Console**: Acompanhar uso de Storage e Firestore
- **Logs**: Verificar erros de upload/download
- **Backup**: Confirmar criação automática de backups

### 🔄 Atualizações
- Manter Firebase SDKs atualizados
- Monitorar mudanças nas APIs do Firebase
- Testar funcionalidades após atualizações

## 📞 Suporte
Para dúvidas ou problemas:
1. Verificar logs do Firebase Console
2. Testar funcionalidades em modo incógnito
3. Verificar conectividade com Firebase
4. Confirmar configurações de segurança

## 🎯 Benefícios para Produção
- **Segurança**: Arquivos protegidos em servidores da Google
- **Confiabilidade**: Backup automático e múltiplas camadas de proteção
- **Escalabilidade**: Suporte a muitos usuários simultâneos
- **Manutenibilidade**: Sistema robusto e fácil de manter
- **Experiência do Usuário**: Interface intuitiva e responsiva

---

**Desenvolvido com ❤️ para gerenciar viagens de amigos de forma segura e eficiente!** 