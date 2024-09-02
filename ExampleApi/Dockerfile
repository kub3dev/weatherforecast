FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5069

ENV ASPNETCORE_URLS=http://+:5069

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["ExampleApi/ExampleApi.csproj", "ExampleApi/"]
RUN dotnet restore "ExampleApi/ExampleApi.csproj"
COPY . .
WORKDIR "/src/ExampleApi"
RUN dotnet build "ExampleApi.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "ExampleApi.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ExampleApi.dll"]
