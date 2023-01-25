FROM alpine:3.8 as builder
LABEL maintainer="<diego-rincon@outlook.com>"
LABEL version="4.5.2"

ENV JAVA_XMX 8G
ENV ANNOTATORS tokenize,ssplit,parse
ENV TIMEOUT_MILLISECONDS 60000
ENV THREADS 5
ENV MAX_CHAR_LENGTH 100000
ENV PORT 9000
ENV VERSION 4.5.2

RUN apk update && \
    apk add git wget openjdk8-jre-base py2-pip py2-curl

WORKDIR /opt

RUN wget https://nlp.stanford.edu/software/stanford-corenlp-${VERSION}.zip 
RUN unzip stanford-corenlp-${VERSION}.zip
RUN rm *.zip

EXPOSE $PORT

CMD java -Xmx$JAVA_XMX -cp "/opt/stanford-corenlp-${VERSION}/*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer -port $PORT -timeout $TIMEOUT_MILLISECONDS -threads $THREADS -maxCharLength $MAX_CHAR_LENGTH -quiet True -annotators $ANNOTATORS
