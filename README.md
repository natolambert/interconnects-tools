# Interconnects Tools for Multimodal Blogging!

Watch an example video [here](https://www.youtube.com/watch?v=0i0aBJGgtpk)!

This takes a markdown file that you would normally convert to HTML for Substack or upload elsewhere and convert it directly into `*.mp3` files for podcasts and `*.mp4` files for YouTube to tap into new audiences. It uses two state-of-the-art generative AI tools/systems:
1. Elevenlabs.io's new multilingual models for audio generation.
2. OpenAI's DALLE3 for fun images.

It costs a few dollars a post if you publish monthly. I'm on the $22/month Elevenlabs plan based on my Substack wordcount and each post costs $2 to 8 of DALLE API credits (0.08 per widescreen image).

Expand your audience and have some fun! You can use my [Elevenlabs referral link](elevenlabs.io/?from=partnerbruce1418) if you're going to try this out!

![Multimodal blog reality sketch](https://github.com/natolambert/interconnects-tools/assets/10695622/096b2f55-f14f-4484-b780-1cfb0bc898ee)

**Notes on using generated audio**: 
In my distribution, I make it clear that it is made via AI, you should do the same.
Second, from the [11labs documentation](https://help.elevenlabs.io/hc/en-us/articles/13313778519057-Are-there-any-restrictions-on-what-voices-I-can-upload-for-voice-cloning-), all audio is watermarked:
> All audio generated by our model will be watermarked, so that it can be instantly traced back to us;

Second, you cannot use my generated voice per 11labs terms and how it is created. 
My API key in the git history is no longer valid, of course.

## Installation
Install from `requirements.txt`
```
conda create -n media python=3.10
conda activate media
```

Right now this is not on pypi, run as following:
```
git clone https://github.com/natolambert/interconnects-tools
cd interconnects-tools
```

### API Keys
To set the OpenAI API key, add the following ([from the docs](https://platform.openai.com/docs/quickstart?context=python)) to your `bashrc`:
```
export OPENAI_API_KEY='your-api-key-here'
```
Similarly, for 11labs:
```
export ELELABS_API_KEY='your-api-key-here'
```
Additionally, the audio/visual tools I used require `ffmpeg`.
**Blogs using these tools**:
(open an issue to be featured!)

| Blog            | Podcast Link                           | YouTube Link                                   |
|-----------------|----------------------------------------|------------------------------------------------|
| [interconnects](https://www.interconnects.ai/) | https://podcast.interconnects.ai/ | https://www.youtube.com/@interconnects |


## Example usage
This is designed to work with the following data format (note, it is exactly as exported from Notion as markdown for an individual post):

```
scripts/
source/
└─- post-title/
|   |-- post-title-name.md
|   └- post-title-name/
|       | img0.png
|       | ...
|       └─ imgN.png
| ...
└─- post-title-two/
| ...
```

### Config
Generate the config file (that contains the paragraphs etc)
```
python scripts/create-config.py source/test-post/ --date="24 December 2023"
```
The image paths can be wrong if you change them on your local machine post export, double check!

*Note: it is recommended to skim the config and combine things like lists, otherwise generation is split into many more parts and needlessly across images at times.*

### Audio 
Base usage is as follows.
```
python scripts/tts.py --input=source/test-post/
```
Audio generation returns descriptions for youtube / podcast with chapters. E.g.:
```
----------------------------------
Printing podcast chapter versions (does not include `see figure` audio):
----------------------------------
Interconnects year in review: 2023
The core themes of ML and the blog this year. What changes in 2024.
This is AI generated audio with Python and 11Labs. Source code can be found here: https://github.com/natolambert/interconnects-tools
Original post: https://www.interconnects.ai/p/TODO

00:00 Interconnects year in review: 2023
01:45 Brief 2024 predictions
03:37 Top posts of the year
05:07 Trends
05:11 RLHF capabilities and understanding
07:09 Open LLM ecosystem progress
08:38 LLM techniques pieces
10:12 Model releases
11:15 Moats
11:44 State of ML opinion pieces
12:33 Understanding reward/preference models
13:01 Wrap up
```

**Non-default usage**:
More paths will need to be passed to the `tts.py` script in the case that you're not using the same file structue and voice.
```
python scripts/tts.py --input=source/your-post/ --elelabs_voice='your_generative_id' 
```
Optionally, add `--farewell_audio` and `--figure_audio` to add a farewell to every post or to tell the audience to look at the figure during the video.

Audio add music + outro (not currently using this, need to think about music more):
```
python experimental/add-music.py --input=audio/20231129-synthetic.mp3
```

### Images
```
python scripts/ttv-generate.py --input=source/test-post/
```

### Video
```
python scripts/ttv-merge.py --input=source/test-post/
```

## TODO list
Keeping note of features I want to add in a lazy manner:
* Adding list of figures to podcast shownotes + link because inserting them is hard.
* Verify that the try-except block in `tts.py` works as intended.
