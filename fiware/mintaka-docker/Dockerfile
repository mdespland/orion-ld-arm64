FROM maven:3.6.3-jdk-11 AS maven-build
ARG VERSION=0.0.1

COPY . /usr/src/
WORKDIR /usr/src
RUN mvn versions:set -DnewVersion=$VERSION
RUN mvn clean install -DskipTests

FROM openjdk:11-jdk-slim
COPY --from=maven-build /usr/src/target/mintaka-0.0.1.jar .
CMD ["java", "-jar", "mintaka-0.0.1.jar"]