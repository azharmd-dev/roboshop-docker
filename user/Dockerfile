FROM node:20.19.5-alpine3.22 AS build 
WORKDIR /opt/server
COPY package.json .
COPY server.js .
RUN npm install 

FROM node:20.19.5-alpine3.22
COPY --from=build /opt/server /opt/server
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
ENV MONGO="true" \
    REDIS_URL="redis://redis:6379" \
    MONGO_URL="mongodb://mongodb:27017/users"
RUN chown -R roboshop:roboshop /opt/server
USER roboshop  
CMD [ "node" , "server.js"]