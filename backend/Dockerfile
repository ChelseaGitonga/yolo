FROM node:16-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install --production

COPY . .

FROM node:16-alpine

WORKDIR /app

COPY --from=builder /app ./

EXPOSE 5000

CMD ["npm", "start"]