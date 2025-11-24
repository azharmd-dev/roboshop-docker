FROM python:3.9 AS build
WORKDIR /opt/server
COPY payment.ini .
COPY *.py  .
COPY *.txt .
RUN pip3 install -r requirements.txt
RUN pip3 install uwsgi

FROM python:3.9
WORKDIR /opt/server
COPY --from=build /opt/server /opt/server
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
CMD [ "uwsgi", "--ini", "payment.ini" ]