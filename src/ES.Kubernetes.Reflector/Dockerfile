FROM mcr.microsoft.com/dotnet/aspnet:8.0-noble-chiseled AS base
WORKDIR /app
EXPOSE 25080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["ES.Kubernetes.Reflector/ES.Kubernetes.Reflector.csproj", "ES.Kubernetes.Reflector/"]
RUN dotnet restore "ES.Kubernetes.Reflector/ES.Kubernetes.Reflector.csproj"
COPY . .
WORKDIR "/src/ES.Kubernetes.Reflector"
RUN dotnet build "ES.Kubernetes.Reflector.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "ES.Kubernetes.Reflector.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ES.Kubernetes.Reflector.dll"]