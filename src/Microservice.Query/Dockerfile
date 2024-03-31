FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/sdk:latest AS build-query-env
WORKDIR /app

# Copia el csproj y restaura las dependencias separadamente para aprovechar la caché de las capas del Docker
COPY ./Infra.Data.Query/Infra.Data.Query.csproj ./Infra.Data.Query/
RUN dotnet restore "./Infra.Data.Query/Infra.Data.Query.csproj"

# Copia el resto de los archivos y construye la aplicación
COPY ./Infra.Data.Query/ ./
RUN dotnet publish -c Release -o out

# Genera la imagen de runtime
FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/sdk:latest
WORKDIR /app
COPY --from=build-query-env /app/out .
ENTRYPOINT ["dotnet", "Infra.Data.Query.dll"]
