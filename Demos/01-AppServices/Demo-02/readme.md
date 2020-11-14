# Deploy .NET Core Api

Deploy Api using [GitHub Actions](https://github.com/Azure/actions)

```yaml
name: Deploy FoodApi

on:
  push:
    branches:
      - master

jobs:
  build-and-deploy:
    runs-on: windows-latest

    steps:
      - uses: actions/checkout@master

      - name: Set up .NET Core
        uses: actions/setup-dotnet@v1
        with:
          dotnet-version: "5.0.x"

      - name: Install dependencies
        run: dotnet restore ${{ github.workspace }}/FoodApi/FoodApi.csproj

      - name: Build with dotnet
        run: dotnet build ${{ github.workspace }}/FoodApi/FoodApi.csproj --configuration Release

      - name: dotnet publish
        run: dotnet publish ${{ github.workspace }}/FoodApi/FoodApi.csproj -c Release -o ${{env.DOTNET_ROOT}}/foodapi

      - name: Deploy to Azure Web App
        uses: azure/webapps-deploy@v2
        with:
          app-name: "skillsapideploy"
          slot-name: "production"
          publish-profile: ${{ secrets.AzureAppService_PublishProfile_b314947fbe6546bf8a51d17b6e0c0a6a }}
          package: ${{env.DOTNET_ROOT}}/foodapi
```
