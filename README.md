# Awesome Twitter Action 
An Github Action that tweets new entries on awesome lists.   

[![](https://img.shields.io/twitter/follow/protontypes?style=social)](https://twitter.com/protontypes) [![](https://img.shields.io/twitter/follow/GHAction1?style=social)](https://twitter.com/GHAction1)

## Usage
When the last commit message on the main branch contains a `https` string a tweet with these commit message will be created.

## Github Action
```
name: Send URLs from Commit Message as Tweets
on: 
  push:
   branches:
      - main
jobs:
  tweet:
    runs-on: ubuntu-latest
    if: "contains(github.event.head_commit.message, 'https')"     
    steps:
      - name: Extract Line with URL as Tweet Message  
        run: |
              echo -e "${{ github.event.head_commit.message }}" >> tweet
              tweet_message="$(cat tweet | grep -m 1 https*)"
              echo "tweet_message=$tweet_message" >> $GITHUB_ENV
      
      - name: Send Tweet       
        uses: ethomson/send-tweet-action@v1
        with:
          status: ${{ env.tweet_message }}
          consumer-key: ${{ secrets.TWITTER_CONSUMER_API_KEY }}
          consumer-secret: ${{ secrets.TWITTER_CONSUMER_API_SECRET }}
          access-token: ${{ secrets.TWITTER_ACCESS_TOKEN }}
          access-token-secret: ${{ secrets.TWITTER_ACCESS_TOKEN_SECRET }}
```
