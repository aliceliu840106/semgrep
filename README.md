# semgrep
RUN useradd -m -d /userap/azure_runner -s /bin/bash azure_runner

ENV PIP_NO_CACHE_DIR=1 \
    PYTHONUNBUFFERED=1 \
    PIP_TARGET=/userap/azure_runner/.local
RUN --mount=type=secret,id=f_pipconf,target=/etc/pip.conf \
    pip install semgrep==1.156.0 --break-system-packages --target=$PIP_TARGET 

RUN chown -R azure_runner:azure_runner /userap/azure_runner
USER azure_runner
ENV PATH="/userap/azure_runner/.local/bin:$PATH"
ENV PYTHONPATH="/userap/azure_runner/.local:$PYTHONPATH"
RUN export  && semgrep login && semgrep install-semgrep-pro
USER root
CMD ["/bin/sh"]
