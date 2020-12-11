# Awesome Twitter Action 
An Github Action that tweets new entries on awesome lists. 

[![](https://img.shields.io/twitter/follow/GHAction1?style=social)](https://twitter.com/GHAction1)

## Usage
When the last commit message contains a `https` string a tweet with the commit message will be created.  

## Implementation
```
name: Send a Tweet
on: [push]
jobs:
  tweet:
    runs-on: ubuntu-latest
    if: "contains(github.event.head_commit.message, 'https')"     
    steps:
      - uses: ethomson/send-tweet-action@v1
        with:
          status: ${{ github.event.head_commit.message }}
          consumer-key: ${{ secrets.TWITTER_CONSUMER_API_KEY }}
          consumer-secret: ${{ secrets.TWITTER_CONSUMER_API_SECRET }}
          access-token: ${{ secrets.TWITTER_ACCESS_TOKEN }}
          access-token-secret: ${{ secrets.TWITTER_ACCESS_TOKEN_SECRET }}
```
