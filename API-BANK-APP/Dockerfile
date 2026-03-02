# Stage 1: Build stage using Maven + JDK
FROM maven:3.9.6-eclipse-temurin-21 AS builder

WORKDIR /app
# Copy Maven descriptor first (better caching)# Copy Maven descriptor first (better caching)
COPY pom.xml .
# Copy source code
COPY src ./src
# Build the application JAR, skipping tests for faster builds
RUN mvn clean package -DskipTests

# Stage 2: Runtime stage using lightweight JRE
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app
# Create a non-root user for security
RUN addgroup -S bank && adduser -S devesh -G bank

USER devesh
# Copy built JAR from builder stage
COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]