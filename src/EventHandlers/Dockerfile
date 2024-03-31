FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/sdk:latest AS build-env
WORKDIR /app

# Copia el csproj y restaura las dependencias separadamente para aprovechar la caché de las capas del Docker
COPY ./Infra.EventHandler/Infra.EventHandler.csproj ./Infra.EventHandler/
RUN dotnet restore "./Infra.EventHandler/Infra.EventHandler.csproj"

# Copia el resto de los archivos y construye la aplicación
COPY ./Infra.EventHandler/ ./
RUN dotnet publish "./Infra.EventHandler/Infra.EventHandler.csproj" -c Release -o out

# Genera la imagen de runtime
FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/sdk:latest
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "Infra.EventHandler.dll"]
