FROM mcr.microsoft.com/dotnet/sdk:latest AS build-env
WORKDIR /app

# Copia el csproj y restaura las dependencias separadamente para aprovechar la caché de las capas del Docker
COPY ./EventHandlers/Infra.EventHandler.csproj ./EventHandlers/
RUN dotnet restore "./EventHandlers/Infra.EventHandler.csproj"

# Copia el resto de los archivos y construye la aplicación
COPY ./EventHandlers/ ./
RUN dotnet publish "./EventHandlers/Infra.EventHandler.csproj" -c Release -o out

# Genera la imagen de runtime
FROM mcr.microsoft.com/dotnet/aspnet:latest
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "Infra.EventHandler.dll"]
