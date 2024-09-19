FROM alpine:3.8 as builder

ENV JAVA_XMX 8G
ENV ANNOTATORS tokenize,ssplit,parse
ENV TIMEOUT_MILLISECONDS 60000
ENV THREADS 5
ENV MAX_CHAR_LENGTH 100000
ENV PORT 9000
ENV VERSION 4.5.7

LABEL org.opencontainers.image.description="Docker Image wrap the Stanford Core NLP toolkit"
LABEL org.opencontainers.image.name="stanford-corenlp"
LABEL org.opencontainers.image.title="Stanford Core NLP"
LABEL org.opencontainers.image.version="$VERSION"
LABEL org.opencontainers.image.authors="Diego, Rincon-Yanez <rinconyanezd+docker@gmail.com>"
LABEL org.opencontainers.image.licenses="MIT"
LABEL org.opencontainers.image.source="https://github.com/d1egoprog/docker-stanford-corenlp"
LABEL org.opencontainers.image.documentation="https://github.com/d1egoprog/docker-stanford-corenlp"

RUN apk update && \
    apk add git wget openjdk8-jre-base py2-pip py2-curl

WORKDIR /opt

RUN wget https://nlp.stanford.edu/software/stanford-corenlp-${VERSION}.zip 
RUN unzip stanford-corenlp-${VERSION}.zip
RUN rm *.zip

EXPOSE $PORT

CMD java -Xmx$JAVA_XMX -cp "/opt/stanford-corenlp-${VERSION}/*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer -port $PORT -timeout $TIMEOUT_MILLISECONDS -threads $THREADS -maxCharLength $MAX_CHAR_LENGTH -quiet True -annotators $ANNOTATORS
