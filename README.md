# github-actions-snippets
Some snippets of GitHub actions YAML that you can copy and paste!!!!!!

**Why this? Isn’t the selling point of GitHub Actions the fact that you can share and reuse others’ actions??** Yes, but sometimes there are cases where I prefer to just copy and paste YAML code instead of reusing others’ actions:

- Sometimes I want to customize the action’s code without going through the repository forking dance.

- There are [security concerns](https://dev.to/mheap/improve-your-github-actions-security-1im7) regards using others’ actions. Sure we can trust official actions and actions created by big companies, but random strangers on the internet? Hmmm. Even if we strictly use commit hash instead of tag, our actions YAML will be full of Git hashes instead of meaningful versions, and so we lose out on automatic updates. I’m also not sure what’s going to happen if the actions’ repository is deleted or if the owner’s account is suspended or deleted.

## Push local changes back to GitHub

Useful for e.g. [repository that updates itself](https://github.com/dtinth/fresh-react-app), [keeping app screenshots up-to-date](https://github.com/dtinth/timelapse).

```yaml
      - name: Push changes back to GitHub
        run: |
          git add --all \
            && git commit -m "Update $(node -p 'new Date().toJSON()')" \
            || echo "Nothing to commit"
          git push "https://x-access-token:${{ secrets.GITHUB_TOKEN }}@github.com/$GITHUB_REPOSITORY.git" "$GITHUB_REF"
```

## Supporting emoji

Useful for workflows that uses a headless browser to render pages that may contain emojis.

```yaml
      - name: Install Noto fonts and adding emoji support
        run: sudo apt-get install fonts-noto fonts-noto-color-emoji -y
```

## Post to Slack channel

```yaml
      - name: Post to Slack
        run: |
          test -f /tmp/slack.json && curl -X POST -H 'Content-type: application/json' -d @/tmp/slack.json "${{ secrets.SLACK_WEBHOOK_URL }}"
```
