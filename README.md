# Eluwa: A Conversational LoRA for Facebook's OPT 2.7b Architecture

![logo](/ELUWA-LOGO.jpg "baaaaaaaaaaaa")

Eluwa is a fine-tuned LoRA model based on Facebook's OPT 2.7b architecture and trained on the Stanford Alpaca dataset. Eluwa is designed to provide a more conversational and creative experience in question-answering mode compared to the default OPT model. The idea was that OPT was too curt (and frankly, a bit of an asshole) for a model of its size, and that we could finetune it like Alpaca did to Llama. 

It worked! Based on very limited testing, it's about halfway to GPT 3.5. Response times are fast: on my GTX 1080ti + Ryzen 3600,it generates between 1.14 tokens/s and 3.77 tokens/s.

```
### Q: Tell me about yourself.
A: I am an AI that can help you with all kinds of tasks. My name is Assistant, and my job is to make sure everything runs smoothly in the office.

### Q: Write me a story about a Paladin in a dark forest.	
A: A paladin? What do you mean by "paladin"? Is it a hero from a fantasy novel? No, he's not. He is just someone who helps people when they need him most. He has a great heart, but sometimes his heart gets too big for its own good.

### Q: Describe this person.	
Well, he wears armor made out of gold and has many magical powers. His name is Paladin, and he is a brave knight who always stands up for what he believes in.
```
Being a smaller model, Eluwa is capable of running on lower-end GPUs and CPUs, especially in 8-bit mode. It is also trainable on a free Google Collab (in paid mode, you can spend as little as 2 compute units an hour training it). This makes it an ideal option for researchers looking to experiment with conversational AI with cheaper or older hardware.

***

## Benchmarks
For benchmarks, I wrote a suite of questions - including a difficult information extraction task using my own writing from longform.watchdog.team. I then pitted OPT 2.7b (in 8 bit mode), OPT 2.7b (8bit) with Eluwa, and GPT 3.5 against each other. Scores and judging were done by GPT-4. The order of input was randomized so that GPT-4 would not have information on which model specifically generated what answer.

Here's the [link to the evaluation spreadsheet](https://docs.google.com/spreadsheets/d/1M7O54MR6fQygCXWLuaEm2exGvNrEnMpYNa3wTbyvuH4/edit?usp=sharing).

Despite Eluwa being not too different from OPT 2.7b, it often does a lot better than default OPT2.7b at accuracy and coherency. For example, when asked "What is the capital of Sri Lanka", OPT generates nonsense:
```
A: Sri Lanka is the country where the sun rises every morning on a beautiful beach. It has many beaches, but only one city - Colombo. (The sun sets at night.)
```
While Eluwa correctly says,
```
A: Colombo.
```

Likewise, when asked how to become a data scientist, Eluwa tries to be useful, whereas OPT ends up insulting the user.

While admittedly hasty and unscientific, the testing gives a very basic idea of conversational ability, creativity, and information extraction from conversation history. According to GPT-4, Eluwa scores 42/90 (46.7%), compared to OPT-default's 23/90 (25.6%) and GPT 3.5's 72/90 (80%). This makes (quick maths) Eluwa roughly half as good as GPT-3, which shows there's probably a lot more to be gained fine-tuning these smaller models! 

Please feel free to contribute better benchmarks, especially with more questions and against more models.


***

## Using Eluwa

I used [oobabooga's text generation UI](https://colab.research.google.com/drive/1rkLx0oI8pbix0EznjYeaLDqPoMHdw0x8?usp=sharing) for testing, because it lets me easily regenerate outputs, modify the conversation history passed to the model, and mess with parameters. 

To load Eluwa, download [OPT 2.7b from Huggingface](https://huggingface.co/facebook/opt-2.7b) and download both the .bin and .json file from the /model folder on this Github. Follow the instructions on the text generation UI repository to figure out where the model goes and how to load a LoRA. Eluwa goes in the /loras folder. 

## Training and notes

Training Eluwa is a straightforward process. It is essentially Facebook's GPT-like OPT 2.7b model, loaded in 8-bit and trained using [Stanford's Alapaca dataset] (https://github.com/tatsu-lab/stanford_alpaca). Use the [Colab notebook here](https://colab.research.google.com/drive/1rkLx0oI8pbix0EznjYeaLDqPoMHdw0x8?usp=sharing). I've written notes in there on what the functions do. 

When loaded thusly, OPT 2.7b gives us 5242880 trainable params out of a total 2656839680 (trainable%: 0.19733520390662038).

In training Eluwa, I made some interesting notes. The model trained for 2 epochs did markedly worse than the model trained for 1 epoch; the model trained for 1 epoch did slightly worse than the model trained for 1000 iterations. 500 iterations didn't do so well, eiither. Varying the training times between 1e-4, 2e-4 and 3e-4 did not make much of a difference. 

Ultimately the 1000 iter model was better and is the default here. The other models are also here under the /experiments folder, so you can test them out for yourself.

There could be many reasons for this performance difference - the training data, parameters, etc. Please try it for yourself and see what you can come up with!    

## Why "Eluwa"?

Well, the whole thing was inspiration from Alpaca, which is a LoRA based on Llama. Others adopted the trend (Cabrita, Vicuna etc). Now, in Sri Lanka, we don't have llamas (at least, I've never seen any), but we do have goats. Goats are spectacular animals. In Ragama I once beheld a goat fighting a pack of stray dogs (and winning). Then it came for me. I hit it on the head with my umbrella, whereupon which it ate the umbrella and chased me the length and breadth of the entire village. 

If you can't beat em, join em. "Eluwa" means goat. Goats are fearsome, versatile, and double as the essential ingredient in mutton rolls. Everything in the known universe is either a goat, or not a goat. They're not as nice as llamas or alpacas, but they'll do.

## License

Facebook's OPT has [its own license. Please read it here.](https://github.com/facebookresearch/metaseq/blob/main/projects/OPT/MODEL_LICENSE.md)
Alpaca is licensed for research use only. The dataset is CC BY NC 4.0 (allowing only non-commercial use) and they note that models trained using the dataset should not be used outside of research purposes. 

Eluwa, therefore, is only for research and non-commercial use, under CC BY NC 4.0. Go experiment with it, but don't use it commercially. 
