---
title: "自動化部署 Hugo 部落格到 GitHub Pages"
date: "2021-03-27T14:00:00+08:00"
draft: false
slug: "deploy-hugo-blog-to-github"
tags: ["Hugo", "GitHub Actions"]
post_keywords: "Hugo,Blog,Github,Github Pages,Github Actions"
---

接續[以 Hugo 建立部落格](../build-a-blog-with-hugo)，Hugo 在其文件中提到了多種 [Hosting and Deployment](https://gohugo.io/hosting-and-deployment/) 方式。若你選擇將 source code 放在 GitHub 上，那麼你可以直接考慮將部落格放在 GitHub Pages 上，並使用 GitHub Actions 自動化所有部署和更新的流程。

<!--more-->

## Step 1: Create a Project on GitHub for the Blog

在 GitHub 上建立部落格 repository，並將 source code 都推上去。

不該推的東西請加到 `.gitignore` 中

- `resources`
- `.DS_Store`

## Step 2: 建立 GitHub Pages

如果你想建立 GitHub Pages，請按照[說明](https://pages.github.com/)在帳號下建立一個名為 `your-username.github.io` 的 repository。

## Step 3: 部署準備

#### Create GitHub token

在 [GitHub Tokens Page](https://github.com/settings/tokens) 建立 `personal access token`，權限選擇 `repo` 和 `workflow`。建立完成後，請複製 token。

#### Add Access Token as Secrets

在部落格 repo 的 `Settings > Secrets` 頁面上，建立一 `Repository secrets`。

- `Name`: 可取你想要的
- `Value`: 貼上剛剛複製的 token

## Step 4: Create A GitHub Action

- 在部落格 repo 建立 `.github/workflows` 資料夾
- 資料夾下新增 `main.yml`，貼上以下 action 設置

```yaml
name: CI
on: push
jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - name: Git checkout
        uses: actions/checkout@v2

      - name: Update theme
        # (Optional) If you have the theme added as submodule, you can pull it and use the most updated version
        run: git submodule update --init --recursive

      - name: Setup hugo
        uses: peaceiris/actions-hugo@v2
        # or change to a version you prefer
        with:
          hugo-version: "0.80.0"

      - name: Build
        # remove --minify tag if you do not need it
        # docs: https://gohugo.io/hugo-pipes/minification/
        run: hugo --minify

      # replace TOKEN and username to your own settings
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          personal_token: ${{ secrets.TOKEN }}
          external_repository: <username>/<username>.github.io
          publish_dir: ./public
          # keep_files: true
          user_name: <username>
          user_email: <username@email.com>
          publish_branch: main
          # cname: example.com
```

## Step 5: Push to GitHub Remote

push 到 remote 後，你將會在 repo 的 `Actions` 頁面看到 action 的進度。等待 action 成功後，就可以在 `https://<username>.github.io/` 看到部落格囉！

---

## Reference

- [GitHub Actions](https://docs.github.com/en/actions)
- [Hugo: Deploy Static Site using GitHub Actions](https://ruddra.com/hugo-deploy-static-page-using-github-actions/)
