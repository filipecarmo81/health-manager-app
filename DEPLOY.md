# Guia de Deploy - Sistema AUSTA Hospital

## Visão Geral

Este guia fornece instruções detalhadas para fazer o deploy do Sistema de Gestão AUSTA Hospital em diferentes plataformas de hospedagem.

## Pré-requisitos

- Node.js 18+ instalado
- npm ou yarn
- Conta na plataforma de deploy escolhida
- Acesso ao ERP TASY (para produção)

## Preparação para Deploy

### 1. Build do Projeto

```bash
# Instalar dependências
npm install

# Criar build de produção
npm run build
```

### 2. Configuração de Ambiente

Crie um arquivo `.env.production` na raiz do projeto:

```bash
# .env.production
REACT_APP_TASY_API_URL=https://api.austahospital.com/tasy
REACT_APP_ENVIRONMENT=production
REACT_APP_VERSION=1.0.0
```

## Deploy na Vercel

### Opção 1: Deploy via CLI

```bash
# Instalar Vercel CLI
npm i -g vercel

# Login na Vercel
vercel login

# Deploy
vercel --prod
```

### Opção 2: Deploy via GitHub

1. Faça push do código para um repositório GitHub
2. Conecte o repositório na Vercel
3. Configure as variáveis de ambiente
4. Deploy automático será feito

### Configuração Vercel (vercel.json)

```json
{
  "version": 2,
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/static-build",
      "config": {
        "distDir": "dist"
      }
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/index.html"
    }
  ],
  "env": {
    "REACT_APP_TASY_API_URL": "@tasy_api_url",
    "REACT_APP_ENVIRONMENT": "production"
  }
}
```

## Deploy no Render

### 1. Conectar Repositório

1. Acesse [render.com](https://render.com)
2. Conecte seu repositório GitHub
3. Escolha "Static Site"

### 2. Configurações

- **Build Command**: `npm run build`
- **Publish Directory**: `dist`
- **Node Version**: 18

### 3. Variáveis de Ambiente

Adicione no painel do Render:
- `REACT_APP_TASY_API_URL`
- `REACT_APP_ENVIRONMENT`

## Deploy no Netlify

### Via CLI

```bash
# Instalar Netlify CLI
npm install -g netlify-cli

# Login
netlify login

# Deploy
netlify deploy --prod --dir=dist
```

### Via Interface Web

1. Arraste a pasta `dist` para netlify.com
2. Configure domínio personalizado
3. Adicione variáveis de ambiente

## Deploy no GitHub Pages

### 1. Instalar gh-pages

```bash
npm install --save-dev gh-pages
```

### 2. Configurar package.json

```json
{
  "homepage": "https://seuusuario.github.io/health-manager-app",
  "scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  }
}
```

### 3. Deploy

```bash
npm run deploy
```

## Deploy em Servidor Próprio

### 1. Preparar Servidor

```bash
# Instalar Node.js e nginx
sudo apt update
sudo apt install nodejs npm nginx

# Instalar PM2 para gerenciamento de processos
npm install -g pm2
```

### 2. Configurar Nginx

```nginx
# /etc/nginx/sites-available/austa-hospital
server {
    listen 80;
    server_name seu-dominio.com;
    root /var/www/austa-hospital/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### 3. Deploy

```bash
# Clonar repositório
git clone https://github.com/seuusuario/health-manager-app.git
cd health-manager-app

# Instalar dependências e build
npm install
npm run build

# Copiar arquivos para nginx
sudo cp -r dist/* /var/www/austa-hospital/

# Ativar site
sudo ln -s /etc/nginx/sites-available/austa-hospital /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

## Configuração de SSL

### Usando Certbot (Let's Encrypt)

```bash
# Instalar certbot
sudo apt install certbot python3-certbot-nginx

# Obter certificado
sudo certbot --nginx -d seu-dominio.com

# Renovação automática
sudo crontab -e
# Adicionar: 0 12 * * * /usr/bin/certbot renew --quiet
```

## Monitoramento e Logs

### 1. Configurar Analytics

Adicione Google Analytics ou similar:

```javascript
// src/utils/analytics.js
import ReactGA from 'react-ga4'

export const initGA = () => {
  ReactGA.initialize('G-XXXXXXXXXX')
}

export const logPageView = () => {
  ReactGA.send({ hitType: 'pageview', page: window.location.pathname })
}
```

### 2. Error Tracking

Configure Sentry ou similar:

```javascript
// src/utils/errorTracking.js
import * as Sentry from '@sentry/react'

Sentry.init({
  dsn: 'YOUR_SENTRY_DSN',
  environment: process.env.REACT_APP_ENVIRONMENT
})
```

### 3. Performance Monitoring

```javascript
// src/utils/performance.js
export const measurePerformance = (name, fn) => {
  const start = performance.now()
  const result = fn()
  const end = performance.now()
  
  console.log(`${name} took ${end - start} milliseconds`)
  return result
}
```

## Backup e Recuperação

### 1. Backup do Código

```bash
# Script de backup
#!/bin/bash
DATE=$(date +%Y%m%d_%H%M%S)
tar -czf "backup_austa_$DATE.tar.gz" /var/www/austa-hospital/
aws s3 cp "backup_austa_$DATE.tar.gz" s3://seu-bucket/backups/
```

### 2. Backup do Banco de Dados

```bash
# Se usando banco de dados local
pg_dump austa_db > backup_db_$(date +%Y%m%d).sql
```

## Troubleshooting

### Problemas Comuns

1. **Erro 404 em rotas**
   - Verificar configuração do servidor para SPA
   - Adicionar fallback para index.html

2. **Variáveis de ambiente não carregam**
   - Verificar se começam com `REACT_APP_`
   - Rebuild após mudanças

3. **Problemas de CORS**
   - Configurar proxy no desenvolvimento
   - Configurar headers CORS no backend

4. **Performance lenta**
   - Ativar compressão gzip
   - Configurar cache de assets
   - Otimizar imagens

### Logs Úteis

```bash
# Logs do Nginx
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log

# Logs do PM2
pm2 logs

# Logs do sistema
sudo journalctl -u nginx -f
```

## Segurança

### 1. Headers de Segurança

```nginx
# Adicionar ao nginx
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header X-Content-Type-Options "nosniff" always;
add_header Referrer-Policy "no-referrer-when-downgrade" always;
add_header Content-Security-Policy "default-src 'self' http: https: data: blob: 'unsafe-inline'" always;
```

### 2. Rate Limiting

```nginx
# Configurar rate limiting
http {
    limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;
    
    server {
        location /api/ {
            limit_req zone=api burst=20 nodelay;
        }
    }
}
```

## Manutenção

### 1. Atualizações

```bash
# Script de atualização
#!/bin/bash
cd /var/www/austa-hospital
git pull origin main
npm install
npm run build
sudo systemctl reload nginx
```

### 2. Monitoramento de Saúde

```bash
# Script de health check
#!/bin/bash
curl -f http://localhost/health || exit 1
```

### 3. Limpeza de Logs

```bash
# Rotação de logs
sudo logrotate /etc/logrotate.d/nginx
```

## Checklist de Deploy

- [ ] Código testado em ambiente de desenvolvimento
- [ ] Variáveis de ambiente configuradas
- [ ] Build de produção criado
- [ ] SSL configurado
- [ ] Backup realizado
- [ ] Monitoramento ativo
- [ ] DNS configurado
- [ ] Performance testada
- [ ] Segurança verificada
- [ ] Documentação atualizada

## Suporte

Para suporte técnico:
- Email: suporte@austahospital.com
- Documentação: [docs.austahospital.com](https://docs.austahospital.com)
- Issues: [GitHub Issues](https://github.com/seuusuario/health-manager-app/issues)

---

*Guia de Deploy - Sistema AUSTA Hospital*
*Versão: 1.0.0 | Data: Julho 2025*

