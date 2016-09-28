FROM node:latest

MAINTAINER Doug Wells

ENV NODE_ENV=production
ENV PORT=3000

COPY . /var/www
WORKDIR /var/www

VOLUME ["/var/www", "/logs"]

RUN npm install

EXPOSE $PORT

ENTRYPOINT ["npm", "start"]
