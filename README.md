name: Generate snake animation

on:
  schedule:
    - cron: "0 0 * * *"   # every day at 00:00 UTC
  workflow_dispatch:       # lets you run it by hand from the Actions tab
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - name: Generate snake SVGs from the contribution graph
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-snake.svg
            dist/github-snake-space.svg?palette=github-dark&color_snake=#FFD400&color_dots=#161B33,#1E3A8A,#2563EB,#38BDF8,#7DD3FC

      - name: Push the SVGs to the "output" branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
