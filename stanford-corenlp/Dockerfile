FROM alpine:3.8 as builder
MAINTAINER Diego Rincon-Yanez <diego-rincon@outlook.com>

ENV JAVA_XMX 8G
ENV ANNOTATORS tokenize,ssplit,parse
ENV TIMEOUT_MILLISECONDS 60000
ENV THREADS 5
ENV MAX_CHAR_LENGTH 100000
ENV PORT 9000
ENV VERSION 4.1.0

RUN apk update && \
    apk add git wget openjdk8-jre-base py2-pip py2-curl

# install latest CoreNLP release
WORKDIR /opt

RUN wget https://nlp.stanford.edu/software/stanford-corenlp-${VERSION}.zip 
RUN unzip stanford-corenlp-${VERSION}.zip
RUN rm *.zip

EXPOSE $PORT

CMD java -Xmx$JAVA_XMX -cp "/opt/stanford-corenlp-${VERSION}/*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer -port $PORT -timeout $TIMEOUT_MILLISECONDS -threads $THREADS -maxCharLength $MAX_CHAR_LENGTH -quiet True -annotators $ANNOTATORS
