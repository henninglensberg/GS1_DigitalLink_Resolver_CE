FROM node:15.1.0-alpine3.12

ENV TZ=Europe/London
# These two environment variables must point to the K8s DNS Service name for Mongo and Build Dync servers
ENV MONGODBCONN="mongodb://gs1resolver:gs1resolver@resolver-mongo-service"
ENV PORT=80

# Enter the fully qualified domain name of your implementation of Resolver, which will be used in
# certain response HTTP headers. Include the "https://" or "http://" prefix depedning on your implementation.
# NB. This value is only used as a string in the "Link:" HTTP response header. Do not include an end forward-slash!
ENV RESOLVER_FQDN="https://id.gs1.org"

RUN mkdir /resolver
COPY templates /resolver/templates
COPY package-lock.json /resolver
COPY package.json /resolver

WORKDIR /resolver
RUN npm install && npm audit fix

COPY src /resolver/src
RUN date > /resolver/src/builddatetime.txt

# Run Node (not NPM) to start the server.
# If the usual practice of 'npm start' is used, any SIGTERM arriving to stop the
# container in a controlled manner is instead absorbed by NPM and the container
# keeps going until it is forced-stopped.
CMD ["node","/resolver/src/index.js"]
