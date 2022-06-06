FROM --platform=linux/arm64v8 eclipse-mosquitto:latest AS builder
WORKDIR /tmp/

# Create user and password for nodes
RUN echo 'sensor:changeme'> passwordfile
RUN mosquitto_passwd -U passwordfile

FROM --platform=linux/arm64v8 eclipse-mosquitto:latest

# Add websocket support (alongside existing MQtt socket support)
RUN echo 'listener 1883' >> /mosquitto/config/mosquitto.conf
RUN echo 'listener 1884' >> /mosquitto/config/mosquitto.conf
RUN echo 'protocol websockets' >> /mosquitto/config/mosquitto.conf

# Edit the config file to enable the default user
RUN echo 'user mosquitto' >> /mosquitto/config/mosquitto.conf

# Add password file
COPY --from=builder /tmp/passwordfile /etc/mosquitto/

RUN echo '# Password Configuration' >> /mosquitto/config/mosquitto.conf
RUN echo 'per_listener_settings true' >> /mosquitto/config/mosquitto.conf
RUN echo 'allow_anonymous false' >> /mosquitto/config/mosquitto.conf
RUN echo 'password_file /etc/mosquitto/passwordfile' >> /mosquitto/config/mosquitto.conf
