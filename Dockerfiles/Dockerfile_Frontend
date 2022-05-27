FROM alpine:3.14

ENV NODE_VERSION 18.2.0

WORKDIR /app

COPY package.json .

RUN npm install

COPY . /app

EXPOSE 3000

CMD [ "npm", "start" ]