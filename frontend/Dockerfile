FROM node:26 AS builder

WORKDIR /app

COPY package*.json .

ENV VITE_USER_SERVICE_URL=
ENV VITE_ORDER_SERVICE_URL=

RUN npm install

COPY . .

RUN npm run build

FROM nginx:latest

COPY --from=builder /app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
EXPOSE 443

CMD [ "nginx", "-g", "daemon off;"]
