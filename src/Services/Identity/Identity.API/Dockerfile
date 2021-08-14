FROM mcr.microsoft.com/dotnet/core/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src/Services/Identity/Identity.API
COPY . .
RUN dotnet restore "Identity.API.csproj"
RUN dotnet publish --no-restore -c Release -o /app

FROM build AS publish
# >>>>>>>>>>>>>>>>>>>>>>>>>>>>
FROM base AS harden
RUN echo "Updating and Patching" && \
apt-get update -yq && apt-get -yq upgrade

COPY scripts/ /hardening/

RUN echo "Hardening" && \
  chmod +x /hardening/* && \
  /hardening/remove_unwanted_packages.sh && \
  /hardening/remove_extra_files.sh
# <<<<<<<<<<<<<<<<<<<<<<<<<<<
FROM harden AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Identity.API.dll"]
