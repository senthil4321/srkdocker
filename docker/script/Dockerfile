FROM busybox
COPY ./init.sh /
RUN ["chmod", "+x", "/init.sh"]
ENV name Senthil Kumar
ENTRYPOINT ["sh","init.sh"]