FROM mcr.microsoft.com/dotnet/sdk:latest AS build-env
WORKDIR /app

# Copia el csproj y restaura las dependencias separadamente para aprovechar la caché de las capas del Docker
COPY ./Microservice.Query/Infra.Data.Query.csproj ./Microservice.Query/
RUN dotnet restore "./Microservice.Query/Infra.Data.Query.csproj"

# Copia el resto de los archivos y construye la aplicación
COPY ./Microservice.Query/ ./
RUN dotnet publish "./Microservice.Query/Infra.Data.Query.csproj" -c Release -o out

# Genera la imagen de runtime
FROM mcr.microsoft.com/dotnet/aspnet:latest
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "Infra.Data.Query.dll"]
