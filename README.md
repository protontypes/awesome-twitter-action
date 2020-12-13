# Awesome Twitter Action 
An GitHub Action that tweets new entries on awesome lists. The goal of this project is to inform readers about new projects on an awesome list in an easy way.

# Implementation
Check if the last commit message on the main branch contains `https` and creates a tweet based on the corresponding line. The action is intended to publish new entries in an Awesome list, thus making new entries easily accessible to readers.  

[![](https://img.shields.io/twitter/follow/protontypes?style=social)](https://twitter.com/protontypes) [![](https://img.shields.io/twitter/follow/GHAction1?style=social)](https://twitter.com/GHAction1)

## Usage
1. Add the Github Action Script to .github/workflows/tweet.yml
2. Create a Twitter Developer Account.
3. Add the Login data to your project secrets.
4. Activate your Github Actions by pressing the Activate GitHub Action for this Projects button on the Action Tab of your project.
5. Create a Commit Message with an URL. Since the title of a pull request is included in the commit message, contributors can create user-specific Twitter messages.


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
