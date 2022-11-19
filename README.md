# nuxtjs3-project-template

デモ用の [NuxtJS](https://nuxt.com/) のプロジェクトを格納したリポジトリです。

## `quickstart-templates/Azure-for-startups` における利用

[quickstart-templates/Azure-for-startups](https://github.com/quickstart-templates/Azure-for-startups) の「[1-1 Nuxt.js で SPA をサーバーレス環境にデプロイしたい](https://github.com/quickstart-templates/Azure-for-startups/tree/main/1_web-application/1-1_spa-on-serverless)」で利用する場合の補足です。

1. このテンプレートリポジトリを元に新規のリポジトリを作成してください。
2. 「[1-1 Nuxt.js で SPA をサーバーレス環境にデプロイしたい](https://github.com/quickstart-templates/Azure-for-startups/tree/main/1_web-application/1-1_spa-on-serverless)」の「Deploy to Azure」ボタンでデプロイのパラメータ入力画面を開きます。パラメータのうち、下記項目についてはこのリポジトリに合うよう設定をご確認ください。適宜入力したらデプロイします。
   | 設定項目 | 説明 |
   |----|----|
   | Static App Config App Location | `nuxt-app`（デフォルト） |
   | Static App Config App Output Location | `.output/public`（デフォルト） |
   | Static App Config App Build Command | `npm run generate`（デフォルト） |
3. 上記でデプロイ完了後は、静的サイトしかデプロイされていないので、API もデプロイするには、リポジトリに作成されたワークフローファイル（`.github/workflows/azure-static-web-apps-***.yml`）を更新します。下記のように `Azure/static-web-apps-deploy` アクションの `api_location` に `api` ディレクトリを指定して、ワークフローファイルをコミットすると、ワークフローが実行され設定が反映されます。
   ```diff
    jobs:
      build_and_deploy_job:
        # -- 略 --
        steps:
          # -- 略 --
          - name: Build And Deploy
            id: builddeploy
            uses: Azure/static-web-apps-deploy@v1
            with:
              azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_WONDERFUL_ROCK_06D394300 }}
              repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for Github integrations (i.e. PR comments)
              action: "upload"
              ###### Repository/Build Configurations - These values can be configured to match your app requirements. ######
              # For more information regarding Static Web App workflow configurations, please visit: https://aka.ms/swaworkflowconfig
              app_location: "nuxt-app" # App source code path
   -          api_location: "" # Api source code path - optional
   +          api_location: "api" # Api source code path - optional
              output_location: ".output/public" # Built app content directory - optional
              app_build_command: "npm run generate" # Custom build command for app content - optional
              ###### End of Repository/Build Configurations ######
   ```

## 参考

### このリポジトリの構成

このリポジトリと同じ構成を作成するには、下記のコマンド実行で可能です。

```bash
# Create Nuxt 3 project
npx nuxi init nuxt-app

# Create Azure Function project for API
mkdir api
cd api
func init --worker-runtime node --langurage javascript
func new --template "HTTP trigger" --authlevel function --name HttpTrigger1

# Put configuration file
echo "{\"platform\": {\"apiRuntime\": \"node:16\"}}" | jq > nuxt-app/staticwebapp.config.json
```
