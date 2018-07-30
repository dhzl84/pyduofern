ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8

# Install requirements for add-on
RUN apk update
RUN apk add --no-cache python3 py-pip git && pip3 install pyduofern==0.23.2


COPY run.sh /
RUN chmod a+x /run.sh
CMD [ "/run.sh" ]
