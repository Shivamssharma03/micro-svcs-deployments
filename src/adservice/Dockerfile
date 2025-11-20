# ===== BUILD STAGE =====
FROM eclipse-temurin:21-jdk AS builder

WORKDIR /app

COPY ["build.gradle", "gradlew", "./"]
COPY gradle gradle

RUN chmod +x gradlew

# This downloads plugins & deps
RUN ./gradlew downloadRepos || true

COPY . .
RUN ./gradlew installDist

# ===== RUNTIME STAGE =====
FROM eclipse-temurin:21-jre

WORKDIR /app
COPY --from=builder /app .

EXPOSE 9555
ENTRYPOINT ["/app/build/install/hipstershop/bin/AdService"]
