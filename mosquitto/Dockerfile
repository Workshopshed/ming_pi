FROM --platform=linux/arm eclipse-mosquitto:latest

# Add websocket support (alongside existing MQtt socket support)
RUN echo 'listener 1883' >> /mosquitto/config/mosquitto.conf
RUN echo 'listener 1884' >> /mosquitto/config/mosquitto.conf
RUN echo 'protocol websockets' >> /mosquitto/config/mosquitto.conf

