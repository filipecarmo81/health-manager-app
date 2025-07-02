# Sistema de Gestão AUSTA Hospital

Um aplicativo web moderno desenvolvido para gestores da área da saúde, oferecendo uma interface intuitiva para monitoramento de indicadores operacionais e financeiros em tempo real.

## 📋 Visão Geral

O Sistema de Gestão AUSTA Hospital é uma solução web responsiva que permite aos gestores acompanhar indicadores críticos de múltiplas unidades de saúde do grupo AUSTA. O sistema oferece uma visão consolidada de métricas operacionais e financeiras, facilitando a tomada de decisões estratégicas.

### Principais Funcionalidades

- **Autenticação Segura**: Sistema de login com validação de credenciais
- **Seleção Multi-Empresa**: Interface para escolha entre diferentes unidades do grupo
- **Dashboard Executivo**: Visualização em tempo real de indicadores-chave
- **Design Responsivo**: Interface otimizada para desktop e dispositivos móveis
- **Integração ERP**: Conectividade com sistema Tasy para dados atualizados

## 🏥 Indicadores Monitorados

### Indicadores Operacionais
- **Taxa de Ocupação de Leitos**: Percentual de ocupação em tempo real
- **Gestão de Contas**: Faturamento enviado e pendente
- **Fluxo de Caixa**: Contas a pagar e receber do dia

### Métricas Financeiras
- **Faturamento Não Enviado**: Valores pendentes fora do prazo
- **Faturamento Enviado**: Contas processadas e enviadas
- **Contas a Pagar**: Valores com vencimento no dia
- **Contas a Receber**: Previsão de recebimentos diários

## 🏢 Empresas do Grupo

O sistema suporta múltiplas unidades:

| Unidade | Tipo | Especialização |
|---------|------|----------------|
| Hospital A | Hospital | Atendimento geral |
| Hospital B | Hospital | Atendimento geral |
| Clínica X | Clínica | Especialidades médicas |
| Clínica Y | Clínica | Especialidades médicas |
| Laboratório Z | Laboratório | Análises clínicas |

## 🚀 Tecnologias Utilizadas

### Frontend
- **React 18**: Biblioteca principal para interface de usuário
- **React Router**: Gerenciamento de rotas e navegação
- **Tailwind CSS**: Framework de estilização utilitária
- **Shadcn/UI**: Componentes de interface pré-construídos
- **Lucide React**: Biblioteca de ícones moderna
- **Vite**: Ferramenta de build e desenvolvimento

### Ferramentas de Desenvolvimento
- **Node.js 18+**: Ambiente de execução JavaScript
- **npm**: Gerenciador de pacotes
- **ESLint**: Linting e qualidade de código

## 📦 Instalação e Configuração

### Pré-requisitos
- Node.js 18.0 ou superior
- npm 8.0 ou superior
- Git

### Passos de Instalação

1. **Clone o repositório**
   ```bash
   git clone https://github.com/seu-usuario/health-manager-app.git
   cd health-manager-app
   ```

2. **Instale as dependências**
   ```bash
   npm install --legacy-peer-deps
   ```

3. **Execute o ambiente de desenvolvimento**
   ```bash
   npm run dev
   ```

4. **Acesse o aplicativo**
   - Abra o navegador em `http://localhost:5173`
   - Use qualquer e-mail e senha válidos para fazer login

## 🔧 Scripts Disponíveis

```bash
# Desenvolvimento
npm run dev          # Inicia servidor de desenvolvimento

# Build
npm run build        # Gera build de produção
npm run preview      # Visualiza build de produção

# Qualidade de código
npm run lint         # Executa ESLint
```

## 🏗️ Arquitetura do Sistema

### Estrutura de Pastas
```
health-manager-app/
├── public/
│   ├── logo.png           # Logo da AUSTA Hospital
│   └── vite.svg
├── src/
│   ├── components/
│   │   ├── ui/            # Componentes base (shadcn/ui)
│   │   ├── Login.jsx      # Componente de autenticação
│   │   └── Dashboard.jsx  # Dashboard principal
│   ├── App.jsx            # Componente raiz
│   ├── App.css            # Estilos globais
│   └── main.jsx           # Ponto de entrada
├── package.json
└── README.md
```

### Fluxo de Navegação
1. **Tela de Login** (`/login`)
   - Validação de credenciais
   - Redirecionamento automático

2. **Dashboard** (`/dashboard`)
   - Seleção de empresa
   - Visualização de indicadores
   - Opção de logout

### Gerenciamento de Estado
- **React Hooks**: useState para estado local
- **Props**: Comunicação entre componentes
- **React Router**: Estado de navegação

## 🎨 Design System

### Paleta de Cores
Baseada na identidade visual da AUSTA Hospital:

- **Primária**: `#00d4d4` (Ciano AUSTA)
- **Secundária**: `#a0a0a0` (Cinza AUSTA)
- **Sucesso**: `#10b981` (Verde)
- **Alerta**: `#f59e0b` (Laranja)
- **Erro**: `#ef4444` (Vermelho)

### Tipografia
- **Fonte Principal**: Inter, system-ui, sans-serif
- **Hierarquia**: h1-h6 com escalas definidas
- **Legibilidade**: Contraste otimizado para acessibilidade

### Componentes
- **Cards**: Containers para indicadores
- **Botões**: Estados hover e focus definidos
- **Formulários**: Validação visual integrada
- **Dropdown**: Seleção de empresas

## 📱 Responsividade

O sistema foi desenvolvido com abordagem mobile-first:

### Breakpoints
- **Mobile**: < 768px
- **Tablet**: 768px - 1024px
- **Desktop**: > 1024px

### Adaptações
- **Grid responsivo**: Ajuste automático de colunas
- **Navegação móvel**: Interface otimizada para touch
- **Tipografia escalável**: Tamanhos adaptativos

## 🔌 Integração com ERP Tasy

### Endpoints Simulados
O sistema está preparado para integração com o ERP Tasy através de APIs REST:

```javascript
// Exemplo de estrutura de dados esperada
{
  "ocupacaoLeitos": 85,
  "faturamentoNaoEnviado": 125000,
  "faturamentoEnviado": 890000,
  "contasPagar": 45000,
  "contasReceber": 78000
}
```

### Configuração de API
Para conectar com o ERP real, modifique o arquivo `Dashboard.jsx`:

1. Substitua a função `generateMockData` por chamadas HTTP
2. Configure endpoints no arquivo de configuração
3. Implemente tratamento de erros e loading states

## 🔒 Segurança

### Autenticação
- **Validação de formulário**: Campos obrigatórios
- **Estado de sessão**: Controle de acesso às rotas
- **Logout seguro**: Limpeza de estado

### Boas Práticas Implementadas
- **Sanitização de inputs**: Prevenção de XSS
- **Rotas protegidas**: Redirecionamento automático
- **Validação client-side**: Feedback imediato

## 🚀 Deploy

### Opções de Hospedagem

#### Vercel (Recomendado)
1. Conecte o repositório GitHub à Vercel
2. Configure as variáveis de ambiente
3. Deploy automático a cada push

#### Render
1. Conecte o repositório ao Render
2. Configure o comando de build: `npm run build`
3. Defina a pasta de saída: `dist`

#### Netlify
1. Arraste a pasta `dist` para o Netlify
2. Configure redirects para SPA
3. Defina variáveis de ambiente

### Variáveis de Ambiente
```bash
# Exemplo de configuração
VITE_API_BASE_URL=https://api.austahospital.com
VITE_APP_VERSION=1.0.0
```

## 📊 Monitoramento e Analytics

### Métricas Recomendadas
- **Performance**: Core Web Vitals
- **Uso**: Google Analytics ou similar
- **Erros**: Sentry ou Bugsnag
- **Uptime**: Pingdom ou UptimeRobot

## 🤝 Contribuição

### Padrões de Código
- **ESLint**: Configuração padrão React
- **Prettier**: Formatação automática
- **Commits**: Conventional Commits

### Processo de Desenvolvimento
1. Fork do repositório
2. Criação de branch feature
3. Desenvolvimento e testes
4. Pull request com descrição detalhada

## 📞 Suporte

### Contatos
- **Desenvolvimento**: equipe-dev@austahospital.com
- **Suporte Técnico**: ti@austahospital.com
- **Gestão**: gestao@austahospital.com

### Documentação Adicional
- [Guia de Usuário](docs/user-guide.md)
- [API Reference](docs/api-reference.md)
- [Troubleshooting](docs/troubleshooting.md)

## 📄 Licença

Este projeto é propriedade da AUSTA Hospital e destina-se exclusivamente ao uso interno da organização.

---

**Desenvolvido com ❤️ pela equipe AUSTA Hospital**

*Última atualização: Julho 2025*


