# ---------- Build ----------
FROM python:3.9 AS build
WORKDIR /opt/server
COPY requirements.txt .
RUN pip3 install --user -r requirements.txt
COPY payment.ini .
COPY *.py .

# ---------- Runtime ----------
FROM python:3.9-slim-bookworm
EXPOSE 8080
WORKDIR /opt/server

RUN apt-get update && apt-get install -y \
    libxml2 \
    libxslt1.1 \
    libpcre3 \
    libssl3 \
    && apt-get clean

RUN groupadd -r roboshop && \
    useradd -r -g roboshop roboshop && \
    chown -R roboshop:roboshop /opt/server

COPY --from=build /root/.local /root/.local
COPY --from=build /opt/server /opt/server

ENV PATH="/root/.local/bin:${PATH}"

ENV CART_HOST="cart" \
    CART_PORT="8080" \
    USER_HOST="user" \
    USER_PORT="8080" \
    AMQP_HOST="rabbitmq" \
    AMQP_USER="roboshop" \
    AMQP_PASS="roboshop123"

USER roboshop
CMD ["uwsgi", "--ini", "payment.ini"]
