FROM elasticsearch:6.7.1

LABEL maintainer="RunnerLee <runnerleer@gmail.com>"

ARG ELASTICSEARCH_VERSION=6.7.1

RUN elasticsearch-plugin install \https://github.com/medcl/elasticsearch-analysis-pinyin/releases/download/v${ELASTICSEARCH_VERSION}/elasticsearch-analysis-pinyin-${ELASTICSEARCH_VERSION}.zip \
    && elasticsearch-plugin install -b https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v${ELASTICSEARCH_VERSION}/elasticsearch-analysis-ik-${ELASTICSEARCH_VERSION}.zip

