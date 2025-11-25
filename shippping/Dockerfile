FROM maven:3.9.11 AS build
WORKDIR /opt/server
COPY pom.xml .
COPY src ./src
RUN mvn clean package && \
    mv target/shipping-1.0.jar shipping.jar

FROM eclipse-temurin:17-jre-alpine
WORKDIR /opt/server
RUN addgroup -S roboshop && \ 
    adduser -S roboshop -G roboshop && \
    chown -R roboshop:roboshop /opt/server
ENV CART_ENDPOINT="cart:8080" \
    DB_HOST=mysql
COPY --from=build /opt/server /opt/server/
USER roboshop  
ENTRYPOINT [ "java", "-jar", "shipping.jar" ]