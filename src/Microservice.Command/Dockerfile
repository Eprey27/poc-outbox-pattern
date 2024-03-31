FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/sdk:latest AS build-command-env
WORKDIR /app

# Copia el csproj y restaura las dependencias separadamente para aprovechar la caché de las capas del Docker
COPY ./Infra.Data.Command/Infra.Data.Command.csproj ./Infra.Data.Command/
RUN dotnet restore "./Infra.Data.Command/Infra.Data.Command.csproj"

# Copia el resto de los archivos y construye la aplicación
COPY ./Infra.Data.Command/ ./
RUN dotnet publish -c Release -o out

# Genera la imagen de runtime
FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/sdk:latest
WORKDIR /app
COPY --from=build-command-env /app/out .
ENTRYPOINT ["dotnet", "Infra.Data.Command.dll"]
