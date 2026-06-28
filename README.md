name: Generate Snake Animation

# This workflow generates an animated SVG of your GitHub contribution graph
# ("snake eating your contributions") and commits it to an `output` branch.
# Your README points to: https://raw.githubusercontent.com/<you>/<you>/output/github-contribution-grid-snake.svg
#
# SETUP (one-time):
#   1. Place this file at .github/workflows/snake.yml in your profile repo
#      (the repo must be named exactly the same as your GitHub username, e.g. "Mehakie19").
#   2. Go to: Repo Settings → Actions → General → Workflow permissions
#      → select "Read and write permissions" → Save.
#   3. Go to the Actions tab → select "Generate Snake Animation" → "Run workflow" (manual first run).
#   4. After that, it runs automatically every day at the schedule below.

on:
  schedule:
    - cron: "0 0 * * *"   # daily at midnight UTC
  workflow_dispatch: {}     # allows manual trigger from the Actions tab
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - name: Generate snake animation SVGs
        uses: Platane/snk@v3
        id: snake-gif
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark

      - name: Push generated files to the "output" branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
