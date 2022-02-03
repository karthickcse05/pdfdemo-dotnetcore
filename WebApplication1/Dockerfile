#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.
FROM surnet/alpine-wkhtmltopdf:3.8-0.12.5-full as wkhtmltopdf
FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["WebApplication1/WebApplication1.csproj", "WebApplication1/"]
RUN dotnet restore "WebApplication1/WebApplication1.csproj"
COPY . .
WORKDIR "/src/WebApplication1"
RUN dotnet build "WebApplication1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApplication1.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# <wkhtmltopdf>
# Install dependencies for wkhtmltopdf
RUN apk add --no-cache \
  libstdc++ \
  libx11 \
  libxrender \
  libxext \
  libssl1.0 \
  ca-certificates \
  fontconfig \
  freetype \
  ttf-dejavu \
  ttf-droid \
  ttf-freefont \
  ttf-liberation \
  ttf-ubuntu-font-family \
  font-noto \
  font-adobe-100dpi 

RUN apk add --no-cache --virtual .build-deps msttcorefonts-installer \
  && update-ms-fonts \
  && fc-cache -f \
  && apk del .build-deps


RUN apk --no-cache --repository http://nl.alpinelinux.org/alpine/edge/testing --allow-untrusted add wqy-zenhei

# Clean up when done
RUN rm -rf /tmp/*

# Copy wkhtmltopdf files from docker-wkhtmltopdf image
COPY --from=wkhtmltopdf /bin/wkhtmltopdf /usr/bin/
COPY --from=wkhtmltopdf /bin/libwkhtmltox* /usr/lib/


ENTRYPOINT ["dotnet", "WebApplication1.dll"]