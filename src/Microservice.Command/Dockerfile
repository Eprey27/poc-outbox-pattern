FROM mcr.microsoft.com/dotnet/sdk:latest AS build-env
WORKDIR /app

# Copia el csproj y restaura las dependencias separadamente para aprovechar la caché de las capas del Docker
COPY ./Microservice.Command/Infra.Data.Command.csproj ./Microservice.Command/
RUN dotnet restore "./Microservice.Command/Infra.Data.Command.csproj"

# Copia el resto de los archivos y construye la aplicación
COPY ./Microservice.Command/ ./
RUN dotnet publish "./Microservice.Command/Infra.Data.Command.csproj" -c Release -o out

# Genera la imagen de runtime
FROM mcr.microsoft.com/dotnet/aspnet:latest
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "Infra.Data.Command.dll"]
