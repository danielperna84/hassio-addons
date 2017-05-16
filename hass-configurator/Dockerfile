FROM %%BASE_IMAGE%%

# Add env
ENV LANG C.UTF-8

# Setup base
RUN apk add --no-cache python3 git
RUN pip3 install GitPython

# Copy data
COPY configurator.py /
COPY run.sh /
RUN chmod a+x /configurator.py
RUN chmod a+x /run.sh

CMD ["/run.sh"]
