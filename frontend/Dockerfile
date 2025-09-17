# frontend/Dockerfile (production)
FROM node:18-alpine AS build
WORKDIR /app

# copy package files first for caching
COPY package*.json ./
RUN npm ci

# copy source and build
COPY . .
# allow injecting API URL at build time
ARG VITE_API_URL
ENV VITE_API_URL=$VITE_API_URL
RUN npm run build

# runtime: nginx serves the built files
FROM nginx:stable-alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
