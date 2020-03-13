# github-actions-snippets
Some snippets of GitHub actions YAML that you can copy and paste, old school style.

**Why this? Isn’t the selling point of GitHub Actions the fact that you can share and reuse others’ actions??** Yes, but sometimes there are cases where I prefer to just copy and paste YAML code instead of reusing others’ actions:

- Sometimes I want to customize the action’s code without going through the repository forking dance.

- There are [security concerns](https://dev.to/mheap/improve-your-github-actions-security-1im7) regards using others’ actions. Sure, we can trust official actions and actions created by big companies, but random strangers on the internet? Hmmm. Even if we strictly use commit hash instead of tag, our actions YAML will be full of Git hashes instead of meaningful versions, and so we lose out on automatic updates. I’m also not sure what’s going to happen if the actions’ repository is deleted or if the owner’s account is suspended or deleted.

By the way, **the contents you will see below is automatically generated from GitHub Issues**.

- They are **sorted by score**, where 👍 😄 🎉 ❤️ reactions count as upvote and 👎 😕 reactions count as downvote.
  So if you find any snippet helpful, please give a reaction on the corresponding issue!

- Of course, [this is implemented using GitHub Actions](https://github.com/dtinth/github-actions-snippets/blob/master/.github/workflows/update.yml)!

<!-- begin autogen 0b31160502a7d992162afd436db36e96dd00a349bffd9723566d4d9d8c1e44f4 -->

## Table of contents
- [Making emojis render properly in headless browsers](#I2)
- [Commit and push local changes back to GitHub](#I1)
- [Post to a Slack channel](#I3)

## <a name="I2"></a>Making emojis render properly in headless browsers
Useful for workflows that uses a headless browser to render pages that may contain emojis.

```yaml
      - name: Install Noto fonts and add emoji support
        run: sudo apt-get install fonts-noto fonts-noto-color-emoji -y
```

<p align="right">
  <sup>submitted by <a href="https://github.com/dtinth">@dtinth</a> (<a href="https://github.com/dtinth/github-actions-snippets/issues/2">#2</a>)</sup>
</p>

## <a name="I1"></a>Commit and push local changes back to GitHub
Useful for e.g. [repository that updates itself](https://github.com/dtinth/fresh-react-app), [keeping app screenshots up-to-date](https://github.com/dtinth/timelapse).

```yaml
      - name: Push changes back to GitHub
        env:
          GIT_COMMITTER_NAME: Janitor
          GIT_AUTHOR_NAME: Janitor
          EMAIL: repository-janitor[bot]@users.noreply.github.com
        run: |
          git add --all \
            && git commit -m "Update $(node -p 'new Date().toJSON()')" \
            || echo "Nothing to commit"
          git push "https://x-access-token:${{ secrets.GITHUB_TOKEN }}@github.com/$GITHUB_REPOSITORY.git" "$GITHUB_REF"
```

<p align="right">
  <sup>submitted by <a href="https://github.com/dtinth">@dtinth</a> (<a href="https://github.com/dtinth/github-actions-snippets/issues/1">#1</a>)</sup>
</p>

## <a name="I3"></a>Post to a Slack channel
Useful for building alerts on (e.g.) build or test failures, as well as other miscellaneous use cases.

```yaml
      - name: Generate a Slack message
        run: |
          echo '{"text":"Hello world"}' | tee /tmp/slack.json
      - name: Post to Slack
        run: |
          test -f /tmp/slack.json && curl -X POST -H 'Content-type: application/json' -d @/tmp/slack.json "${{ secrets.SLACK_WEBHOOK_URL }}"
```

<p align="right">
  <sup>submitted by <a href="https://github.com/dtinth">@dtinth</a> (<a href="https://github.com/dtinth/github-actions-snippets/issues/3">#3</a>)</sup>
</p>

<!-- end autogen 0b31160502a7d992162afd436db36e96dd00a349bffd9723566d4d9d8c1e44f4 -->
