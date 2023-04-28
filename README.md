# Eluwa: A Conversational LoRA for Facebook's OPT Architecture

![logo](/ELUWA-LOGO.jpg "baaaaaaaaaaaa")


Eluwa is a family of fine-tuned LoRA models based on Facebook's OPT architecture and trained on the Stanford Alpaca dataset. They're designed to provide a more conversational and creative experience in question-answering mode compared to the default OPT model. The idea was that OPT was too curt (and frankly, a bit of an asshole) for a model of its size, and that we could finetune it like Alpaca did to Llama. 

Eluwa comes in 1.3b, 2.7b and 6.7b flavors, each based on the corresponding OPT model. The models can be loaded in -8bit, making this set useful for researchers digging into conversational AI models with older and slower hardware. Response times are fast: on my GTX 1080ti + Ryzen 3600, OPT 2.7 + Eluwa 2.7b generates between 1.14 tokens/s and 3.77 tokens/s.

## Using Eluwa

To load Eluwa, first get OPT:   
https://huggingface.co/facebook/opt-1.3b  
https://huggingface.co/facebook/opt-2.7b  
or https://huggingface.co/facebook/opt-6.7b.  

Then load Eluwa from the Hugginface repo:  
https://huggingface.co/BackyardLabs/Eluwa-1.3b  
https://huggingface.co/BackyardLabs/Eluwa-2.7b  
or https://huggingface.co/BackyardLabs/Eluwa-6.7b.  

You can easily load this with the Huggingface libraries; if you want a UI that can run locally, I recommend [oobabooga's text generation UI](https://github.com/oobabooga/text-generation-webui). It lets you easily regenerate outputs, modify the conversation history passed to the model, and mess with parameters.  Follow the instructions on the text generation UI repository to figure out where the model goes and how to load a LoRA. Eluwa goes in the /loras folder.  

## Training, testing and notes

Training Eluwa is a straightforward process. It is essentially Facebook's GPT-like OPT 2.7b model, loaded in 8-bit and trained using [Stanford's Alapaca dataset](https://github.com/tatsu-lab/stanford_alpaca). This repo contains the training notebook. I've written notes in there on what the functions do. 

Eluwa was tested primarily in the style of Vicuna - using their 80 questions and using GPT-4 to rank the answers. These questions and answers are in this repo under /TestResults/. For other testing (Wikibench, SQUAD) see the Eluwabench notebook in this repo.

For more details, we'll link the paper as soon as it appears on Arxiv.


## Why "Eluwa"?

Well, the whole thing was inspiration from Alpaca, which is a LoRA based on Llama. Others adopted the trend (Cabrita, Vicuna etc). Now, in Sri Lanka, we don't have llamas (at least, I've never seen any), but we do have goats. Goats are spectacular animals. In Ragama I once beheld a goat fighting a pack of stray dogs (and winning). Then it came for me. I hit it on the head with my umbrella, whereupon which it ate the umbrella and chased me the length and breadth of the entire village. 

If you can't beat em, join em. "Eluwa" means goat. Goats are fearsome, versatile, and double as the essential ingredient in mutton rolls. Everything in the known universe is either a goat, or not a goat. They're not as nice as llamas or alpacas, but they'll do.


## License

Facebook's OPT has [its own license. Please read it here.](https://github.com/facebookresearch/metaseq/blob/main/projects/OPT/MODEL_LICENSE.md)
Alpaca is licensed for research use only. The dataset is CC BY NC 4.0 (allowing only non-commercial use) and they note that models trained using the dataset should not be used outside of research purposes. 

Eluwa, therefore, is only for research and non-commercial use, under CC BY NC 4.0. Go experiment with it, but don't use it commercially. 


## Citing
To cite this work, please use:
```
@article{wijeratne2023better,
      title={Better Question-Answering Models on a Budget}, 
      author={Yudhanjaya Wijeratne and Ishan Marikar},
      year={2023},
      eprint={2304.12370},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
```

as well as the original OPT paper:
```
@article{zhang2022opt,
  title={Opt: Open pre-trained transformer language models},
  author={Zhang, Susan and Roller, Stephen and Goyal, Naman and Artetxe, Mikel and Chen, Moya and Chen, Shuohui and Dewan, Christopher and Diab, Mona and Li, Xian and Lin, Xi Victoria and others},
  journal={arXiv preprint arXiv:2205.01068},
  year={2022}
}
```

