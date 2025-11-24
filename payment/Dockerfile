FROM python:3.9-slim AS build
WORKDIR /opt/server

# Copy app files
COPY payment.ini .
COPY *.py .
COPY *.txt .

# Install dependencies
RUN pip3 install --no-cache-dir -r requirements.txt
RUN pip3 install --no-cache-dir uwsgi

# --------------------
# Final runtime image
# --------------------
FROM python:3.9-slim
WORKDIR /opt/server

COPY --from=build /opt/server /opt/server

# Create service user
RUN addgroup --system roboshop && adduser --system --ingroup roboshop roboshop

ENV CART_HOST="cart" \
    CART_PORT="8080" \
    USER_HOST="user" \
    USER_PORT="8080" \
    AMQP_HOST="rabbitmq" \
    AMQP_USER="roboshop" \
    AMQP_PASS="roboshop123"

RUN chown -R roboshop:roboshop /opt/server
USER roboshop

CMD ["uwsgi", "--ini", "payment.ini"]

# FROM python:3.9
# EXPOSE 8080
# WORKDIR /opt/server
# COPY payment.ini .
# COPY *.py .
# COPY requirements.txt .
# RUN pip3 install -r requirements.txt
# ENV CART_HOST="cart" \
#     CART_PORT="8080" \
#     USER_HOST="user" \
#     USER_PORT="8080" \
#     AMQP_HOST="rabbitmq" \
#     AMQP_USER="roboshop" \
#     AMQP_PASS="roboshop123"
# CMD ["uwsgi","--ini","payment.ini"]