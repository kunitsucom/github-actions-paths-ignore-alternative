# GitHub Actions `paths-ignore` Alternative

```yml
      - uses: kunitsucom/github-actions-paths-ignore-alternative@main
        id: paths-ignore
        with:
          paths-ignore: |-
            # substrings of file paths to ignore written in regular expressions
            .github/
            .*\.md
            .*\.log
      - name: "if not skip"
        if: ${{ steps.paths-ignore.outputs.skip != 'true' }}
        run: |
            echo "not skip workflow"
      - name: "if skip"
        if: ${{ steps.paths-ignore.outputs.skip == 'true' }}
        run: |
            echo "skip workflow"
```
