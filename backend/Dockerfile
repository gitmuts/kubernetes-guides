#build command docker build --build-arg env=sit -t gidimuts/adminbackend:sit .
FROM maven:3.5.3-jdk-8-alpine AS builder
WORKDIR /workspace/app
COPY pom.xml .
RUN mvn dependency:go-offline

COPY src src
COPY wsdl wsdl
RUN mvn -DskipTests=true package
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf ../*.jar)

FROM openjdk:8-jdk-alpine
ARG env=dev
VOLUME /tmp
VOLUME /app/config

COPY soauat.cer /app/your_cerfificate.cer
RUN cd $JAVA_HOME/jre/lib/security && keytool -keystore cacerts -storepass changeit -noprompt -trustcacerts -importcert -alias soauat2020 -file /app/your_cerfificate.cer

ARG DEPENDENCY=/workspace/app/target/dependency
COPY --from=builder ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=builder ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=builder ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-Dspring.config.location=/app/","-cp","app:app/lib/*","com.gitmuts.yourapp.MainApplicationClass"]
