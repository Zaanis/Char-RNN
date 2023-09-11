# Char-RNN
A PyTorch implementation of [char-rnn](https://github.com/karpathy/char-rnn) for character-level text generation.This is my implementation of [Char-RNN.Pytorch](https://github.com/spro/char-rnn.pytorch)

## Updates from Original
fixed train.py error, updated ine 66 to loss.item()/args.chunk_len
added except statement in helpers.py (line 26-27) in case a character cannot be unidecoded
added a notebook to manually check which characters in the text file cannot be unidecoded
added my training data text files (python java) also have a c file that was not used, as well as the .pt file generated

### To be fixed
Even after removing all none unidecodable characters, training for long epochs (over 5000) sometimes resulted in this error:

RuntimeError: The expanded size of the tensor (200) must match the existing size (199) at non-singleton dimension 0.  Target sizes: [200].  Tensor sizes: [199]

Error stems from line 44 of train.py, so I've added a try except block and it seems to have solved the issue, not sure if it is the right way of doing so. Originally thought this was becuase a character is not unidecodable, but after removing those characters, still runs into the error.

## Training

Download [this Shakespeare dataset](https://raw.githubusercontent.com/karpathy/char-rnn/master/data/tinyshakespeare/input.txt) (from the original char-rnn) as `shakespeare.txt`.  Or bring your own dataset &mdash; it should be a plain text file (preferably ASCII).

Run `train.py` with the dataset filename to train and save the network:

```
> python train.py shakespeare.txt

Training for 2000 epochs...
(... 10 minutes later ...)
Saved as shakespeare.pt
```
After training the model will be saved as `[filename].pt`.

### Training options

```
Usage: train.py [filename] [options]

Options:
--model            Whether to use LSTM or GRU units    gru
--n_epochs         Number of epochs to train           2000
--print_every      Log learning rate at this interval  100
--hidden_size      Hidden size of GRU                  50
--n_layers         Number of GRU layers                2
--learning_rate    Learning rate                       0.01
--chunk_len        Length of training chunks           200
--batch_size       Number of examples per batch        100
--cuda             Use CUDA
```

## Generation

Run `generate.py` with the saved model from training, and a "priming string" to start the text with.

```
> python generate.py shakespeare.pt --prime_str "Where"

Where, you, and if to our with his drid's
Weasteria nobrand this by then.

AUTENES:
It his zersit at he
```

### Generation options
```
Usage: generate.py [filename] [options]

Options:
-p, --prime_str      String to prime generation with
-l, --predict_len    Length of prediction
-t, --temperature    Temperature (higher is more chaotic)
--cuda               Use CUDA
```
