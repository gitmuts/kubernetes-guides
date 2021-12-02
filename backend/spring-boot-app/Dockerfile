#build command docker build -t gidimuts/backendapp .
FROM maven:3.8.3-jdk-11 AS builder
WORKDIR /workspace/app
COPY pom.xml .
RUN mvn dependency:go-offline

COPY src src
RUN mvn -DskipTests=true package
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf ../*.jar)

FROM eclipse-temurin:11-alpine

ARG DEPENDENCY=/workspace/app/target/dependency
COPY --from=builder ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=builder ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=builder ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-Dspring.config.location=/app/","-cp","app:app/lib/*","com.gitmuts.backend.BackendApplication"]
