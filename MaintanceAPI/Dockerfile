FROM mcr.microsoft.com/dotnet/aspnet:7.0-focal AS base
WORKDIR /app
EXPOSE 7001

ENV ASPNETCORE_URLS=http://+:7001

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM mcr.microsoft.com/dotnet/sdk:7.0-focal AS build
WORKDIR /src
COPY ["MaintanceAPI.csproj", "./"]
RUN dotnet restore "MaintanceAPI.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "MaintanceAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MaintanceAPI.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MaintanceAPI.dll"]
