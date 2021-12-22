FROM node:17-alpine3.12 as BUILD

RUN mkdir /app

WORKDIR /app

ENV NODE_OPTIONS=--openssl-legacy-provider

COPY package.json .

COPY package-lock.json .

RUN npm install

COPY . .

RUN npm run build



FROM nginx:1.21-alpine

EXPOSE 80

RUN rm -f /usr/share/nginx/html/*

RUN echo "server { listen 80; listen [::]:80; server_name localhost; root /usr/share/nginx/html; location / { try_files \$uri \$uri/ /index.html; } }" > /etc/nginx/conf.d/default.conf

COPY --from=BUILD /app/build/ /usr/share/nginx/html

CMD ["nginx", "-g", "daemon off;"]
