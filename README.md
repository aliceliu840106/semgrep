FROM  python:3.13.3
RUN useradd -m -d /userap/azure_runner -s /bin/bash azure_runner

ENV PIP_NO_CACHE_DIR=1 \
    PYTHONUNBUFFERED=1 \
    PIP_TARGET=/userap/azure_runner/.local 
RUN pip install semgrep==1.156.0 --break-system-packages --target=$PIP_TARGET

RUN chown -R azure_runner:azure_runner /userap/azure_runner 
USER azure_runner 
ENV PATH="/userap/azure_runner/.local/bin:$PATH" 
ENV PYTHONPATH="/userap/azure_runner/.local:$PYTHONPATH" 
RUN export SEMGREP_APP_TOKEN="dff69d4355b8457d6012ebd861f18e0ce0116069caa015e7009855eb4ab39546" && semgrep login && semgrep install-semgrep-pro 
USER root 
CMD ["/bin/sh"]
