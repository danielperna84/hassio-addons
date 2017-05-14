FROM %%BASE_IMAGE%%

# Add version
ENV VERSION %%VERSION%%
ENV LANG C.UTF-8

# Setup base
RUN apk add --no-cache python3
RUN apk add --no-cache git
RUN pip3 install GitPython

# Copy data
COPY configurator.py /
RUN chmod a+x /configurator.py
COPY run.sh /
RUN chmod a+x /run.sh

CMD ["/run.sh"]
