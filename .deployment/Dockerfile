# Stap 1: Gebruik een Node image om eerst de afbeeldingen te optimaliseren
FROM node:20 AS builder

# Maak een werkdirectory
WORKDIR /app

# Kopieer je websitebestanden naar de container
COPY . .

# Installeer image compression tool (imagemin + plugins)
RUN npm install -g imagemin-cli imagemin-mozjpeg imagemin-pngquant imagemin-svgo

# Maak een geoptimaliseerde kopie van de images in een nieuwe map
RUN mkdir -p compressed-img && \
    imagemin img/* --plugin=mozjpeg --plugin=pngquant --plugin=svgo > /dev/null && \
    imagemin img/* --out-dir=compressed-img

# Verwijder originele img-map en vervang door gecomprimeerde versies
RUN rm -rf img && mv compressed-img img


# Stap 2: Gebruik een Nginx image om de geoptimaliseerde site te serveren
FROM nginx:stable-alpine

# Kopieer websitebestanden van de builder stage naar nginx directory
COPY --from=builder /app /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
