# Build
FROM eclipse-temurin:17-jdk as builder

WORKDIR /workspace/app

COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
COPY src src

RUN ./mvnw --batch-mode clean package -DskipTests

# Runner
FROM eclipse-temurin:19-jdk

COPY --from=builder /workspace/app/target/spring-petclinic*.jar /spring-petclinic.jar

ADD https://github.com/aws-observability/aws-otel-java-instrumentation/releases/latest/download/aws-opentelemetry-agent.jar /opt/aws-opentelemetry-agent.jar
ENV JAVA_TOOL_OPTIONS=-javaagent:/opt/aws-opentelemetry-agent.jar

ENTRYPOINT ["java","-jar","/spring-petclinic.jar"]