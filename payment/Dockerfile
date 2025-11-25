# ---------- Build ----------
FROM python:3.9 AS build
WORKDIR /opt/server
COPY payment.ini .
COPY *.py .
COPY requirements.txt .
RUN pip3 install -r requirements.txt

# ---------- Runtime ----------
FROM python:3.9-slim
EXPOSE 8080
WORKDIR /opt/server
RUN groupadd -r roboshop && \
    useradd -r -g roboshop roboshop && \
    chown -R roboshop:roboshop /opt/server
ENV CART_HOST="cart" \
    CART_PORT="8080" \
    USER_HOST="user" \
    USER_PORT="8080" \
    AMQP_HOST="rabbitmq" \
    AMQP_USER="roboshop" \
    AMQP_PASS="roboshop123"
USER roboshop 
CMD ["uwsgi","--ini","payment.ini"]