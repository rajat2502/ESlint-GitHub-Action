# ESlint-GitHub-Action
A Simple GitHub Action that runs the linter(ESlint) on a JavaScript project. This runs ESlint on any file with an extension of .js,.jsx,.ts, and .tsx.

### How to use in my project?

- Copy the below code:
```
on: [push]

jobs:
  build:
    name: Linter
    # This job runs on Linux
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install modules
        run: yarn
      - name: Install ESlint
        run: sudo yarn global add eslint
      - name: Run ESlint
        run: eslint . --ext .js,.jsx,.ts,.tsx --fix
      - name: Check for modified files
        id: git-check
        run: echo ::set-output name=modified::$(if git diff-index --quiet HEAD --; then echo "false"; else echo "true"; fi)
      - name: Push changes
        if: steps.git-check.outputs.modified == 'true'
        run: |
          git config --global user.name 'Rajat Verma'
          git config --global user.email 'rajatverma5885045@gmail.com'
          git remote set-url origin https://x-access-token:${{ secrets.GITHUB_TOKEN }}@github.com/${{ github.repository }}
          git commit -am "lint: file(s) linted"
          git push
```
- Replace the name and email Address with your own email address.
- Create a file in the `.github/workflows` folder of your project.
- Name the file `lint.yml` or any other name (The extension of the file should be .yml or .yaml).
- Make any commit to see it in Action!
