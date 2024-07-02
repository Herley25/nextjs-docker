# Etapa 1: Build da aplicação
FROM node:18-alpine AS builder

# Definir diretório de trabalho
WORKDIR /app

# Instalar as dependências
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* ./
RUN \
  if [ -f yarn.lock ]; then yarn install; \
  elif [ -f package-lock.json ]; then npm install; \
  elif [ -f pnpm-lock.yaml ]; then corepack enable pnpm && pnpm install; \
  else echo "Lockfile not found." && exit 1; \
  fi

# Copiar os arquivos do projeto
COPY . .

# Construa o projeto
RUN npm run build

# Etapa 2: Criar a imagem de produção
FROM node:18-alpine

# Definir diretório de trabalho
WORKDIR /app

# Copiar projeto construído do estágio construtor
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

# Expor a porta
EXPOSE 3000

# Iniciar o servidor de desenvolvimento
CMD ["npm", "start"]
