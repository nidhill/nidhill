name: generate animation

on:
  # എല്ലാ ദിവസവും അർദ്ധരാത്രിയിൽ പ്രവർത്തിപ്പിക്കുക
  schedule:
    - cron: "0 0 * * *" 
  
  # എപ്പോൾ വേണമെങ്കിലും മാനുവലായി പ്രവർത്തിപ്പിക്കാൻ അനുവദിക്കുക
  workflow_dispatch:
  
  # പ്രധാന ബ്രാഞ്ചിലേക്ക് പുഷ് ചെയ്യുമ്പോൾ പ്രവർത്തിപ്പിക്കുക
  push:
    branches:
    - main

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    
    steps:
      # snake animation നിർമ്മിക്കുന്നു
      - name: generate github-contribution-grid-snake.svg
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          
      # നിർമ്മിച്ച ഫയലുകൾ output എന്ന ബ്രാഞ്ചിലേക്ക് പുഷ് ചെയ്യുന്നു
      - name: push github-contribution-grid-snake.svg to the output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
