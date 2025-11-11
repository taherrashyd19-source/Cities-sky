name: Build Cities Skylines 1000 Tiles Mod

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: windows-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup MSBuild
        uses: microsoft/setup-msbuild@v2

      - name: Setup NuGet
        uses: nuget/setup-nuget@v2

      - name: Restore packages (if any)
        run: nuget restore

      - name: Build TileExtender.dll
        run: msbuild /t:Build /p:Configuration=Release /p:TargetFrameworkVersion=v3.5 /p:OutputPath=bin\\Release Deylam.TileExtender.csproj

      - name: List built DLLs
        run: dir /s /b bin\\Release\\*.dll

      - name: Upload DLL artifact
        uses: actions/upload-artifact@v4
        with:
          name: TileExtender-DLL
          path: bin/Release/*.dll
