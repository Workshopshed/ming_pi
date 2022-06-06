FROM --platform=linux/arm64 eclipse-mosquitto:latest AS builder
WORKDIR /tmp/

#Create Random Passphrase
RUN apk add --upgrade mkpasswd
RUN echo date +%s | sha256sum | base64 | head -c 32; echo | mkpasswd > passphrase.txt

# Generate new keys
# From https://community.element14.com/products/roadtest/b/blog/posts/harting-mica-alpine-linux-and-another-mqtt-container-with-certificate-and-tls-ssl#
RUN apk add openssl
RUN openssl genrsa -des3 -passout file:passphrase.txt -out ca.key 2048
RUN openssl req -new -x509 -days 1826 -passin file:passphrase.txt -key ca.key -out ca.crt -subj "/C=GB/ST=London/L=London/O=Global Security/OU=IT Department/CN=example.com"
RUN openssl genrsa -out server.key 2048
RUN openssl req -new -out server.csr -key server.key -subj "/C=GB/ST=London/L=London/O=Global Security/OU=IT Department/CN=example.com"
RUN openssl x509 -req -in server.csr -passin file:passphrase.txt -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 360 

# Create user and password for nodes
RUN echo date +%s | sha256sum | base64 | head -c 32; echo > sensorpass.txt
RUN (echo -n 'sensor:' ; cat sensorpass.txt) > passwordfile
RUN mosquitto_passwd -U passwordfile

FROM --platform=linux/arm64 eclipse-mosquitto:latest

# Add websocket support (alongside existing MQtt socket support)
RUN echo 'listener 1883' >> /mosquitto/config/mosquitto.conf
RUN echo 'listener 1884' >> /mosquitto/config/mosquitto.conf
RUN echo 'protocol websockets' >> /mosquitto/config/mosquitto.conf

# Add keys to image
RUN mkdir /etc/mosquitto
RUN mkdir /etc/mosquitto/ca_certificates
RUN mkdir /etc/mosquitto/certs
COPY --from=builder /tmp/ca.crt /etc/mosquitto/ca_certificates
COPY --from=builder /tmp/server.crt /etc/mosquitto/certs
COPY --from=builder /tmp/server.key /etc/mosquitto/certs

# Edit the config file to enable the default user
RUN echo 'user mosquitto' >> /mosquitto/config/mosquitto.conf

# Edit the config file to point to the keys and to enable the default user
RUN echo '# Key Configuration' >> /mosquitto/config/mosquitto.conf
RUN echo 'capath /etc/mosquitto/ca_certificates/ca.crt' >> /mosquitto/config/mosquitto.conf
RUN echo 'keyfile /etc/mosquitto/certs/server.key' >> /mosquitto/config/mosquitto.conf
RUN echo 'certfile /etc/mosquitto/certs/server.crt' >> /mosquitto/config/mosquitto.conf

# Add password file
COPY --from=builder /tmp/passwordfile /etc/mosquitto/
COPY --from=builder /tmp/sensorpass.txt /etc/mosquitto/

RUN echo '# Password Configuration' >> /mosquitto/config/mosquitto.conf
RUN echo 'per_listener_settings true' >> /mosquitto/config/mosquitto.conf
RUN echo 'allow_anonymous false' >> /mosquitto/config/mosquitto.conf
RUN echo 'password_file /etc/mosquitto/passwordfile' >> /mosquitto/config/mosquitto.conf
