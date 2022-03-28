FROM mcr.microsoft.com/dotnet/sdk:6.0 as build-env
WORKDIR /app

RUN apt-get update
RUN apt-get install -y curl
RUN apt-get install -y libpng-dev libjpeg-dev curl libxi6 build-essential libgl1-mesa-glx
RUN curl -sL https://deb.nodesource.com/setup_lts.x | bash -
RUN apt-get install -y nodejs

# Copy everything and publish the release (publish implicitly restores and builds)
COPY . ./app

RUN dotnet restore ./app/ui.csproj
RUN dotnet publish ./app/ui.csproj -o /app/out --no-self-contained

FROM mcr.microsoft.com/dotnet/aspnet:6.0.3-alpine3.15-arm64v8
WORKDIR /app

RUN apk add --update nodejs
RUN apk add --update npm

EXPOSE 80

COPY --from=build-env /app/out .
ENTRYPOINT [ "dotnet", "/app/ui.dll" ]