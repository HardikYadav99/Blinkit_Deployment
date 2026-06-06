FROM node:20-alpine as builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# Force Vite to bake the relative path into the static JS production bundle
ENV VITE_API_URI=/api
RUN npm run build

FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD [ "nginx", "-g" , "daemon off;"]

#comment to make pipeline work by get method 1.0