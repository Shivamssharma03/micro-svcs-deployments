# ================================
#   BUILD STAGE (.NET 9)
# ================================
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS builder
WORKDIR /src

# Copy only main project file
COPY cartservice.csproj .

# Restore dependencies
RUN dotnet restore cartservice.csproj

# Copy full source
COPY . .

# Remove tests folder so publish won’t include it
RUN rm -rf tests

# Publish a trimmed, single-file executable for linux-x64
RUN dotnet publish cartservice.csproj \
    -c Release \
    -o /app/publish \
    --self-contained true \
    -p:PublishSingleFile=true \
    -p:PublishTrimmed=true \
    -p:TrimMode=full \
    -r linux-x64


# ================================
#   RUNTIME STAGE (.NET 9)
# ================================
FROM mcr.microsoft.com/dotnet/runtime-deps:9.0

WORKDIR /app
COPY --from=builder /app/publish .

ENV ASPNETCORE_URLS=http://0.0.0.0:7070
EXPOSE 7070

ENTRYPOINT ["./cartservice"]
