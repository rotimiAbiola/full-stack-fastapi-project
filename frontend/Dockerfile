FROM node:20-alpine3.19 as build

WORKDIR /app

COPY package*.json .

RUN npm install

COPY . .

EXPOSE 5173

RUN npm run build

FROM nginx:stable-alpine

COPY --from=build /app/dist/ /usr/share/nginx/html

COPY ./nginx.conf /etc/nginx/conf.d/default.conf

