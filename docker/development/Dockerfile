# Dockerfile para Desenvolvimento

# Imagem base
FROM node:18-alpine

# Definir diretório de trabalho
WORKDIR /app

# Instalar dependências
COPY package.json yarn.lock* package-lock.json* pnpm-lock.yaml* ./
RUN \
  if [ -f yarn.lock ]; then yarn install; \
  elif [ -f package-lock.json ]; then npm install; \
  elif [ -f pnpm-lock.yaml ]; then corepack enable pnpm && pnpm install; \
  else echo "Lockfile not found." && exit 1; \
  fi

# Copiar aquivos do projeto
COPY . .

# Expor porta
EXPOSE 3000

# Iniciar o servidor de desenvolvimento
CMD ["npm", "run", "dev"]
