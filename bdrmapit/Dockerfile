FROM python:3.7

RUN pip install --no-cache-dir --upgrade poetry
RUN poetry config virtualenvs.in-project true

WORKDIR /app
COPY extern/ extern/
COPY pyproject.toml pyproject.toml
COPY poetry.lock poetry.lock
RUN poetry install

ENV PATH="/app/.venv/bin:${PATH}"
CMD ["/bin/bash"]
