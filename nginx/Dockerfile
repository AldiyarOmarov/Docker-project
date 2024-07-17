FROM nginx

COPY nginx.conf /etc/nginx/
ARG CHANGE_SOURCE=false
RUN if [ ${CHANGE_SOURCE} = true ]; then \
    echo "Changing package source" \
    && sed -i 's/http:\/\/archive.ubuntu.com/http:\/\/mirrors.aliyun.com/' /etc/apt/sources.list \
;fi

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends openssl bash \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN if [ `grep -c www-data /etc/group` = 0 ]; then \
         adduser -D -H -u 1000 -s /bin/bash www-data; \
    else \
         adduser -D -H -u 1000 -s /bin/bash www-data -G www-data; \
    fi

ADD ./startup.sh /opt/startup.sh
RUN sed -i 's/\r//g' /opt/startup.sh
CMD ["/bin/bash", "/opt/startup.sh"]
EXPOSE 80 443
