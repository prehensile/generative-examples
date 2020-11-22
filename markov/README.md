# Generative Text: Markov chain sample code

The scripts in this folder contain Python code to download source text, train a Markov model (using the [Markovify library](https://github.com/jsvine/markovify)), and generate text from that model.

I'd encourage you to pick through the scripts, maybe even before actually running them, to get an idea of what they're doing. I've tried to put useful comments in there.


## Installation & requirements
This code has been written and tested on Python 3.8.5, but it should work in any reasonably recent version of Python 3 (and maybe event Python 2).

### Installing requirements
The required libraries for this project are specified in `requirements.txt`. If you're not using `virtualenv`, you can install them on the command line like so:
```
pip3 install -r requirements.txt
```

### Using virtualenv
If you're not already familiar with [virtualenv](https://docs.python-guide.org/dev/virtualenvs/#lower-level-virtualenv), I'd recommend using it to keep things tidy between Python projects. You can set up a virtual environment for this project with the following commands:

* Create a new Python 3 virtual environment in the `markov` folder:
    ```
    cd markov
    virtualenv -p python3 .venv
    ```
* Activate the new virtual environment for use:
    ```
    source .venv/bin/activate
    ```
* Install project dependencies (libraries) in the new virtual environment:
    ```
    pip3 install -r requirements.txt
    ```

You'll need to reactivate the virtual environment with the command
``
`source .venv/bin/activate
```
whenever you want to use the libraries installed in it.

## Downloading a training corpus 
This example uses some Raymond Chandler novels downloaded from the web as its source text. In [sources/chandler](sources/chandler), there's a file named [sources.txt](sources/chandler/sources.txt). If you open that in a text editor, you can see that this file contains a list of URLs; these are the source URLs 

If you want to create another corpus from a different set of URLs, you can create another folder in [source](sources) and put another `sources.txt` file in there containing those URLs.

If you already have the text files, you can just place them directly in the folder.

Once you are happy with the URL list for your source(s), you can extract the text from the pages ath those URLs with the following commands:
```
cd sources
python3 fetch_sources.py
```
This will run the [fetch_sources.py](sources/fetch_sources.py) Python script, which steps through all of the folders in [sources](sources), scrapes text from the URLs specified in the `sources.txt` files it finds and saving them in the same folders.

If the source files end up containing a lot of extra stuff besides the text that you want (say, menu items or contents lists), you might want to tidy them up by hand before training the model.

## Training
Once you've got a training corpus downloaded, you can train a model with the following command:
```
python3 train.py sources/chandler chandler.markovify
```
(you'll need to be in the [markov](markov) folder for this; you might need to do `cd..` if you're currently in the `sources` folder)

This command trains a model on all of the text files in [sources/chandler](sources/chandler) and saves the resulting model to a file called `chandler.markovify`. If you've got source texts in some other source folders, you can change references to `chandler` for something else to train those other models.


## Generating
Once you've trained a model, you can generate some text with the following command:
```
python3 generate.py chandler.markovify
```
Once you've trained a model, you can generate some text with the following command:
By default, this will generate 5 sentences. To generate more, you can use the `num-sentences` parameter:
```
python3 generate.py --num-sentences 10 chandler.markovify
``` 

If you've trained a model with a different name, you can change the reference to `chandler.markovify` to fit.

Have fun!