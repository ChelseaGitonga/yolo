FROM node:16-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install --production

COPY . .

RUN npm run build

FROM node:16-alpine

WORKDIR /app

COPY --from=builder /app ./

RUN npm install -g serve

EXPOSE 3000

CMD ["serve","-s","build"]



