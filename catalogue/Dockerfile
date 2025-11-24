# -------------------------
# Stage 1: Build the Node app
# -------------------------
FROM node:20.19.5-alpine3.22 AS build    # Use Node Alpine base for lightweight build

WORKDIR /opt/server                       # Set working directory inside container

COPY package.json .                        # Copy dependencies list
COPY server.js .                           # Copy app code

RUN npm install                            # Install dependencies

# -------------------------
# Stage 2: Run
# -------------------------
FROM node:20.19.5-alpine3.22              # Use a clean runtime image

WORKDIR /opt/server                        # Working directory

COPY --from=build /opt/server /opt/server  # Copy app from build stage

# Create roboshop user & group (non-root best practice)
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop

# App environment variables
ENV MONGO="true" \
    MONGO_URL="mongodb://mongodb:27017/catalogue"

# Give ownership to roboshop user
RUN chown -R roboshop:roboshop /opt/server

USER roboshop                             # Switch to non-root user

CMD [ "node", "server.js" ]               # Start the application


# FROM node:20
# WORKDIR /opt/server
# COPY package.json .
# COPY server.js .
# RUN npm install
# ENV MONGO="true" \
#     MONGO_URL="mongodb://mongodb:27017/catalogue"
# CMD [ "node", "server.js" ]