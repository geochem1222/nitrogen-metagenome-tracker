# Nitrogen Metagenomics Paper Tracker

这是一个面向“水、土壤、沉积物环境中的宏基因组/宏转录组/微生物群落与氮循环”的静态论文追踪网页。默认只使用 Semantic Scholar API，通过 bulk search 回溯文献，并用 Semantic Scholar batch API 回填引用数、参考文献、开放 PDF 和相似文章。

## 研究方向

- 环境介质：soil、water、freshwater、river、stream、lake、reservoir、pond、wetland water、sediment、benthic、groundwater、estuary 等。
- 组学与功能基因：metagenome、metagenomics、metatranscriptome、microbiome、microbial community、functional genes、amoA、nirK、nirS、nosZ、nifH、hzsA、hao、nxrA、narG、napA、nrfA、ureC 等。
- 氮循环过程：nitrogen cycling、nitrification、denitrification、nitrogen fixation、anammox、DNRA、nitrate reduction、ammonia oxidation、nitrite oxidation、nitrous oxide/N2O 等。
- 分类标签：urban、agriculture、forest 只作为筛选标签，不作为默认检索条件。

## 自动更新

`.github/workflows/update-and-deploy.yml` 默认每天 UTC 21:18 运行一次，对应中国时间次日 05:18。它会：

1. 使用 Semantic Scholar bulk search 发现和回溯论文。
2. 用 Semantic Scholar batch API 回填引用数、参考文献数、开放 PDF、代表性参考文献和相似文章。
3. 将新增文献合并到现有 `data/papers.json`。
4. 自动提交数据变化。
5. 将静态网页发布到 `gh-pages` 分支。

建议在 GitHub 仓库 `Settings` -> `Secrets and variables` -> `Actions` 里添加：

```text
SEMANTIC_SCHOLAR_API_KEY=你的 Semantic Scholar API key
CONTACT_EMAIL=你的邮箱
```

## GitHub Pages 发布

1. 新建仓库，例如 `nitrogen-metagenome-tracker`。
2. 把本文件夹内容推送到仓库 `main` 分支。
3. 打开仓库 `Settings` -> `Pages`。
4. Source 选择 `Deploy from a branch`。
5. Branch 选择 `gh-pages`，Folder 选择 `/ (root)`。
6. 到 `Actions` 页面手动运行一次 `Update papers and deploy`。

发布后地址通常是：

```text
https://你的用户名.github.io/nitrogen-metagenome-tracker/
```

## 本地预览

在本文件夹运行：

```bash
python -m http.server 8002
```

然后打开：

```text
http://localhost:8002
```

## 手动更新数据

默认使用 Semantic Scholar：

```bash
python scripts/update_papers.py --retmax 5000 --daily-retmax 800 --cache-days 30 --sources semantic --semantic-search-mode bulk --merge-existing --semantic-enrich-limit 5000 --similar-limit 120 --similar-per-paper 5
```

网页不会在浏览器端直接调用 Semantic Scholar API，因此不会暴露 API key。
