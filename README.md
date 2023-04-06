# Quote Bot Template

The point of this little project is to show how simple it is to get started with technical tools to share valuable content. 

Once you have these skills they can be put to use to maintain a social media presence without having to scroll all day, they can support organizations you are involved with, and this is a doorway into learning how to code.

Also included are tools and instructions for printing text onto images with python, imagemagick and some shell script (including scripts to crop images in bulk).

## Python Quote Bot for Twitter (text)

The twitter quote bots were adapted the script found in [this guide](https://dototot.com/how-to-write-a-twitter-bot-with-python-and-tweepy/). I saved the secrets for the Marshall Rosenberg Quotes account as [encrypted environment variables](https://docs.github.com/en/actions/reference/encrypted-secrets) saved in this repository, which are pulled into a bash shell as part of the action, and then called into the python script. If you copy this code, you'll need to set your own.

Using the GitHub Action workflow files [.github/workflows](/.github/workflows/), the scripts are run on a [regular schedule](https://crontab.guru/):

```yaml
on:
  schedule:
    - cron:  '11 11 * * *'
    - cron:  '22 19 * * *'
```

[Workflow Scheduler/Cron Syntax](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#scheduled-events)

When [quotes.py](quotes.py) runs at 3:33, it pulls a random quote from [quotes.yaml](quotes.yaml) and sends a status update to [@MarshallRsnberg](https://twitter.com/marshallrsnberg) the Marshall Rosenberg Quotes twitter account.

Obviously, you'll have to set it up for your account, on which more detailed instructions are coming.

### The script - quotes.py

The following 14 lines of code are all it takes to send a status update to twitter! Well, you'll have to get your twitter developer account setup and add your keys as environment to this repository... still, it feels pretty empowering.

```python
# Adapted from: https://dototot.com/how-to-write-a-twitter-bot-with-python-and-tweepy/
# Lines beginning with # are comments and ignored by the python interpreter.

import tweepy, yaml, random, os

CONSUMER_KEY = os.environ.get('CONSUMER_KEY')
CONSUMER_SECRET = os.environ.get('CONSUMER_SECRET')
ACCESS_KEY = os.environ.get('ACCESS_KEY')
ACCESS_SECRET = os.environ.get('ACCESS_SECRET')

auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
auth.set_access_token(ACCESS_KEY, ACCESS_SECRET)
api = tweepy.API(auth)

with open('quotes.yaml','r') as file:
    # The FullLoader parameter handles the conversion from YAML
    # scalar values to Python the dictionary format
    quotes = yaml.load(file, Loader=yaml.FullLoader)
    file.close()
    item = random.choice(quotes)
    print(item)
    api.update_status(status=item, source=sourceLabel)
```

### Twitter Image Quote Bot

I printed [quotes](text-to-image/quotes.yaml) onto [images](imgs) using [quimg.py](text-to-image/quimg.py).

[.github/workflows/quimg.yml](/.github/workflows/quimg.yml) sends an image quote status update every 9 hours at the 11th minute.

```yaml
    - cron:  '33 15 * * *'
    - cron:  '11 23 * * *'
```

## Workflow Files

Once you've got your social media secrets saved as environment variables, you can enable the script you want to run by moving the relevant workflow file from [.github/workflows.disabled](.github/workflows.disabled) to [.github/workflows](.github/workflows).

In [Marshall Rosenberg Quotes](https://github.com/danforth-restorative/MBR_Quotes) I've got all of them running at once, so that both facebook and twitter get 2 text quotes and 2 image quotes, every day.

## Cropping Tools

Aspect Ratio Calculator:

https://andrew.hedges.name/experiments/aspect_ratio/

Need Imagemagick for all this stuff:

https://imagemagick.org/index.php

### crop all in directory

http://www.fmwconcepts.com/imagemagick/aspectcrop/index.php

```bash
for f in * ;
./aspectcrop -a 1920:1080 $f cropped.$f
```

### Using Convert 
Aspectcrop only crops proportionally, if you want exact dimensions cropped, use convert.

```bash
for f in * ;   
convert $f -crop 1080x1080+0+0 -gravity center insta.$f
```

## Optimization
### Reduce size of jpg in place

https://stackoverflow.com/questions/50985087/how-to-reduce-the-file-size-on-jpeg-images-in-batch-mac

```bash
mogrify -define jpeg:extent=300kb *.jpg
```

### PngQuant

https://pngquant.org/

## Privacy Policy

This app does not collect or store any user data.

[Privacy Policy for Quote Bot](privacy.md)

---

With Love