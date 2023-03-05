# WS项目部署github-pages

记录参考[官方文档](https://www.jetbrains.com/help/writerside/deploy-docs-to-github-pages.html#search)遇到的一些问题

### image 资源目录配置
![image-web-path.png](image-web-path.png)

修改Writerside/writerside.cfg文件, 按项目目录填写web-path

### 新建工作流配置文件

* step1: 项目目录下新建`.github/workflows/`目录(用于存放github actions工作流文件)

* step2: `.github/workflows/`目录下新建`build-pages.yml`配置文件(命名随意)

* 目录结构如下

* ![Create new_workflow_file](new_workflow_file.png){ width=290 }{border-effect=line}

### 修改配置文件

#### 需要配置的部分
```yaml
env:
  INSTANCE: 'Writerside/b'
  ARTIFACT: 'webHelpB2-all.zip'
  DOCKER_VERSION: '241.15989'
```

env.INSTANCE:
: 可以直接在Writerside的instance配置页面看到instance ID, 这里的ID是b, 所以INSTANCE配置为`'Writerside/b'`
(前面的可能是目录? 默认的话就是Writerside)

: ![instance-id.png](instance-id.png)

env.ARTIFACT:
: 生成的压缩项目名, 格式是`webHelpXX2-all.zip`, 其中`XX`是大写的Instance ID, 这里的ID是b,
所以ARTIFACT配置为`'webHelpB2-all.zip'`

env.DOCKER_VERSION:
: 暂时用默认版本, 不做修改

### 需要添加的部分
需要设置 GITHUB_TOKEN 权限 部署流程中会用到
```yaml
permissions:
  contents: read
  pages: write
  id-token: write
```

### `build-pages.yml` 最终内容 {collapsible="true"}
```yaml
name: Build documentation

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

env:
  INSTANCE: 'Writerside/b'
  ARTIFACT: 'webHelpB2-all.zip'
  DOCKER_VERSION: '241.15989'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Build docs using Writerside Docker builder
        uses: JetBrains/writerside-github-action@v4
        with:
          instance: ${{ env.INSTANCE }}
          artifact: ${{ env.ARTIFACT }}
          docker-version: ${{ env.DOCKER_VERSION }}

      - name: Save artifact with build results
        uses: actions/upload-artifact@v4
        with:
          name: docs
          path: |
            artifacts/${{ env.ARTIFACT }}
            artifacts/report.json
          retention-days: 7
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Download artifacts
        uses: actions/download-artifact@v4
        with:
          name: docs
          path: artifacts

      - name: Test documentation
        uses: JetBrains/writerside-checker-action@v1
        with:
          instance: ${{ env.INSTANCE }}
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    needs: [ build, test ]
    runs-on: ubuntu-latest
    steps:
      - name: Download artifacts
        uses: actions/download-artifact@v4
        with:
          name: docs

      - name: Unzip artifact
        run: unzip -O UTF-8 -qq '${{ env.ARTIFACT }}' -d dir

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Package and upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: dir

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### GitHub action配置
Github项目配置里将 Pages->Build and deployment->Source 修改为GitHub Actions
这样才会去跑我们新建的`.github/workflows/build-pages.yml`流水线

![github-pages-actions.png](github-pages-actions.png)