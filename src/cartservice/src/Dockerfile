# Copyright 2021 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# https://mcr.microsoft.com/product/dotnet/sdk
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:9.0.307-noble@sha256:5643129f0c9f09bab23387c57bfeea1e3e42f47c7370fd9e389a5739e5ef383c AS builder
ARG TARGETARCH
WORKDIR /app
COPY cartservice.csproj .
RUN dotnet restore cartservice.csproj \
    -a $TARGETARCH
COPY . .
RUN dotnet publish cartservice.csproj \
    -p:PublishSingleFile=true \
    -a $TARGETARCH \
    --self-contained true \
    -p:PublishTrimmed=true \
    -p:TrimMode=full \
    -c release \
    -o /cartservice

# https://mcr.microsoft.com/product/dotnet/runtime-deps
FROM mcr.microsoft.com/dotnet/runtime-deps:9.0.11-noble-chiseled@sha256:e17d399464d9978eda3cd7481087e89dee5d6f032a375c0b28f32c6b3c7b2840

WORKDIR /app
COPY --from=builder /cartservice .
EXPOSE 7070
ENV DOTNET_EnableDiagnostics=0 \
    ASPNETCORE_HTTP_PORTS=7070
USER 1000
ENTRYPOINT ["/app/cartservice"]
