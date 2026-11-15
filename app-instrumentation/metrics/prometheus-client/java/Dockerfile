# ---- build stage: compile and shade a fat jar ----
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app
# Resolve dependencies first so this layer caches across source changes.
COPY pom.xml .
RUN mvn -B -q dependency:go-offline
COPY src ./src
RUN mvn -B -q package

# ---- run stage: slim JRE with just the fat jar ----
FROM eclipse-temurin:25.0.5_8-jre@sha256:a640968ae55027b322cedd5f8228b01a84baff0b9e9360e47601c6a111ba1699
WORKDIR /app
COPY --from=build /app/target/app.jar app.jar
# Prometheus /metrics is served on 9100 for Alloy to scrape.
EXPOSE 9100
ENTRYPOINT ["java", "-jar", "app.jar"]
