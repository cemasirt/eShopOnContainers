FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src/Services/Basket
COPY . .
WORKDIR /src/Services/Basket/Basket.API
RUN dotnet restore "Basket.API.csproj"
RUN dotnet publish --no-restore -c Release -o /app

FROM build as unittest
WORKDIR /src/Services/Basket/Basket.UnitTests

FROM build as functionaltest
WORKDIR /src/Services/Basket/Basket.FunctionalTests

FROM build AS publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Basket.API.dll"]
