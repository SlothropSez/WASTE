# WASTE

This is a fork of [Shithead-X](https://github.com/albino/shithead-X) and is very hack-ish, it makes low level changes only

GPT-2 SUPER NEXT GENERATION MACHINE LEARNING irc bot

## Getting started

#Set up a virtual env, I used conda

conda create --name WASTE python=3.8.5 

#Activate the env, then

cd /path/to/WASTE

conda install pytorch

pip install pytorch-lightning==0.8.4

pip install -r requirements.txt

### Building a model

You can use any pytorch GPT-2 model, but for functionality as a chatbot, it's best to train one using existing IRC logs. This way, your chatbot will mimic the culture of the channel you're deploying it in.

This can take a while to get just right - a bit of trial and effort will be worth it here.

You can use anything as the dataset, however, simply collect the text into a .txt (use <|startoftext|> and <|endoftext|> to delimit sections) and you're good to go.

#### Filtering the logs

This is not as simple as it sounds, as any 'unhelpful' input that is not filtered at this stage can cause problems with the model later on. **A simple, well thought-out filter is crucial to training a good model later.** Obvious things to filter include names, timestamps, URLs and bot output - I have included some filter scripts I used as examples in `dump_logs.py` and `filter.sh`.

#### Training the model

I don't actually know anything about natural language processing or artificial intelligence, I just played about with it until I was happy with the result. Training the provided 124M model worked the best for me, although this will require a powerful GPU to train (you can train it on Google's servers if you don't have one at home) and text generation can take a while on a weak CPU.

You may follow [this tutorial](https://colab.research.google.com/drive/15qBZx5y9rdaQSyWpsreMDnTiZ5IlN0zD?usp=sharing) provided by aitextgen's author.

As of this writing (September 2, 2020) you must insert:

!pip install tensorflow==1.15

!pip install pytorch-lightning==0.8.4

Beneath the transformers install.

If you want to use the 355M and larger models, you can follow [this](https://colab.research.google.com/drive/1VLG8e7YSEwypxU-noRNhsv5dW4NfTGce) colab notebook. Then copy the trained model to your gdrive where you can download it. Once downloaded, extract the files. Install [gpt-2-simple](https://github.com/minimaxir/gpt-2-simple) and tensorflow 1.15. Navigate to your environment's "Scripts" folder and copy the file named "transformers-cli" and paste it into the folder containing the folder "checkpoint" that was in the checkpoint_run1.tar, then create a folder called "pytorch". Now, still in the folder containing the folder "checkpoint"
, run:

python transformers-cli convert --model_type gpt2 --tf_checkpoint checkpoint/run1 --pytorch_dump_output pytorch --config checkpoint/run1/hparams.json

This will convert the model from gpt-2-simple into a pytorch model suitable for use in aitextgen. Navigate to the "pytorch" folder and you should have two files, "config.json" and "pytorch_model.bin", move these files to a folder called "trained_model" within the WASTE directory. 

### Configuration

Copy `config.default.ini` to `config.ini` and edit accordingly. Run `gpt2_bot.py` to start.

The bot will only connect using SSL to servers with valid certificates. This is a feature. If a network can't set up SSL properly, it doesn't deserve to be graced by shithead-X. If you really want to change this behaviour, edit `irc.py`.

## Commands

* .shitposting - controls the % of messages the bot will reply to
```
< user> .shitposting 1.5
< shithead-X> OK
```

* .temp - controls the temperature. This is a value which has something to do with randomness, I don't really know.
```
< user> .temp
< shithead-X> Current temperature: 0.9
< shithead-X> Regular temperature range is 0.7 - 1.0 (higher values are crazier)
< user> .temp 0.7
< shithead-X> New temperature is 0.7
```

* .ignore - make the bot totally ignore a user (useful for other bots)
```
< user> .ignore bot
< shithead-X> Now ignoring bot
```

* .unignore - removes a user from the ignore list
```
< user> .unignore bot
< shithead-X> No longer ignoring bot
```

* .ping - checks that shithead-X is still alive
```
< user> .ping
< shithead-X> user: Pong!
```

## FAQ

**Q**: Can you send me a model to use?    
**A**: No.

**Q**: Can you help me with something else?    
**A**: Yes. Open an issue, or contact me via email or IRC.

## Caveats

* I don't know anything about artifical intelligence  
* I don't know anything about Python  
* This is a new project and it probably has some issues, I'm running it in a small channel myself so I will endeavour to get it working as well as I can. Bug reports welcome, PRs even welcomer.

## Thanks

Thanks to Max Woolf for writing aitextgen, the team at OpenAI for making GPT-2 available to the public, and meiscoffee for helping train the model on his GPU.

## Ethics

Please read the [ethics](https://github.com/minimaxir/aitextgen#ethics) section of aitextgen's README if you intend to seriously deploy shithead-X. In short, while it's funny to trick people with Markov chain bots, GPT-2 is 'real' AI and you should probably let people know they are talking to a robot.

## License

CC0/Public Domain; for more information please see the `LICENSE` file.
