FROM node:17.1.0-alpine as ui-builder

RUN mkdir /app

WORKDIR /app

# ENV PATH /usr/src/app/node_modules/.bin:$PATH

RUN wget --quiet https://github.com/freqtrade/frequi/archive/refs/tags/0.4.7.tar.gz -O /tmp/ui.tar.gz \
    && tar xf /tmp/ui.tar.gz -C /app --strip 1 \
    && rm /tmp/ui.tar.gz

RUN apk add --update --no-cache python3 g++ make\
    && yarn \
    && apk del python3 g++ make

RUN yarn global add @vue/cli

COPY . /app

# The below flag should be removed, it's an incompatibility between
# webpack and node17
RUN NODE_OPTIONS=--openssl-legacy-provider yarn build

FROM nginx:1.21.4-alpine
COPY  --from=ui-builder /app/dist /etc/nginx/html
COPY  --from=ui-builder /app/nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
CMD ["nginx"]
