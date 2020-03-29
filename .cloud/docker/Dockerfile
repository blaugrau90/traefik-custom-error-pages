# jekyll
FROM jekyll/builder:latest as jekyll
WORKDIR /tmp
COPY . /tmp
RUN chown -R jekyll:jekyll /tmp 
RUN jekyll build

# nginx
FROM nginx:alpine as nginx

LABEL traefik.enable=true
LABEL traefik.http.routers.catchall_router.entrypoints=web
LABEL traefik.http.routers.catchall_router.rule=HostRegexp(`{catchall:.*}`)
LABEL traefik.http.routers.catchall_router.priority=1
LABEL traefik.http.routers.catchall_router.service=catchall_service@docker
LABEL traefik.http.routers.catchall_router_ssl.entrypoints=websecure
LABEL traefik.http.routers.catchall_router_ssl.rule=HostRegexp(`{catchall:.*}`)
LABEL traefik.http.routers.catchall_router_ssl.priority=1
LABEL traefik.http.routers.catchall_router_ssl.tls=true
LABEL traefik.http.routers.catchall_router_ssl.service=catchall_service@docker
LABEL traefik.http.middlewares.errorpage.errors.status=400-599
LABEL traefik.http.middlewares.errorpage.errors.service=catchall_service@docker
LABEL traefik.http.middlewares.errorpage.errors.query=/{status}.html
LABEL traefik.http.services.catchall_service.loadbalancer.server.port=80

COPY .cloud/nginx/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=jekyll /tmp/_site/ /var/www/public/
