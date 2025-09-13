# Stage 1: Build the application with Maven
# Use a valid base image tag for Maven with Java 24
FROM maven:3.9-eclipse-temurin-24 AS build

# Set the working directory
WORKDIR /app

# Copy only the pom.xml to leverage Docker's layer caching
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy the rest of your source code
COPY src ./src

# Package the application
RUN mvn clean package -DskipTests

# Stage 2: Create the final, lightweight runtime image
# Use a valid JRE 24 image for the final stage
FROM eclipse-temurin:24-jre

WORKDIR /app

# Copy the JAR file from the 'build' stage
COPY --from=build /app/target/patient-service-0.0.1-SNAPSHOT.jar app.jar

# Expose the port the application runs on
EXPOSE 8080

# The command to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]