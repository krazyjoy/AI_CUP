## Workflow
![architecture](https://github.com/krazyjoy/AI_CUP/blob/master/images/workflow.PNG)

## Meaning of Melspectrogram
Studies have shown that humans do not perceive frequencies on a linear scale. 
We are better at detecting differences in lower frequencies than higher frequencies. 
For example, we can easily tell the difference between 500 and 1000 Hz, but we will hardly be able to tell a difference between 10,000 and 10,500 Hz, even though the distance between the two pairs are the same.
In 1937, Stevens, Volkmann, and Newmann proposed a unit of pitch such that equal distances in pitch sounded equally distant to the listener. 
This is called the mel scale. We perform a mathematical operation on frequencies to convert them to the mel scale. 
- reference: [understanding the mel spectrogram](https://medium.com/analytics-vidhya/understanding-the-mel-spectrogram-fca2afa2ce53)
![frequency-to-melscale](https://github.com/krazyjoy/AI_CUP/blob/master/images/mel_scale.PNG)


## Melspectrogram Feature Process
<p align="center">
    load audio <br>
    $\quad \quad \downarrow$ <br>
    convert to melspectrogram <br>
    $\quad \quad\downarrow$ <br>
    (<code>n_mels: 256, fmin=0, fmax=14000</code>) <br>
    $\quad \quad\downarrow$ <br>
    frequency map to decibel format <br>
    (<code>np.abs(stft)</code>) <br>
    $\quad \quad\downarrow$ <br>
    resize to <code>(256, 512)</code>   <br>
    <code>"from skimage.transform import resize"</code><br>
    $\quad \quad\downarrow$ <br>
    stack to 3 channels (for cnn)  <br>
    $\quad \quad\downarrow$ <br>
    <code>np.stack((stft_db),(stft_db),(stft_db))</code> <br>
    $\quad \quad\downarrow$ <br>
    append each sample to list <br> 
    $\quad \quad \downarrow \quad$ <br>
    <code>(nsamples, 3, 256, 512)</code>
</p>


![a sample outlook of melspectrogram](https://github.com/krazyjoy/AI_CUP/blob/master/images/melspectrogram.PNG)


## Medical Record Processing
all columns $\rightarrow$ subset of relevant columns
|          |       |        |                      |
|----------|-------|--------|----------------------|
|    'ID'  | 'Sex' |  'Age' | 'Narrow pitch range' |
|'Decreased volume' | 'Fatigue' | 'Dryness' | 'Lumping' | 
|'heartburn' | 'Choking' | 'Eye dryness' | 'PND' |
| 'Smoking' | 'PPD' | 'Drinking' | 'frequency'|
| 'Diurnal pattern' | 'Onset of dysphonia' | 'Noise at work' | 'Occupational vocal demand' |
|'Head injury' | 'CVA'|'Voice handicap index - 10'| 'Disease category' |

- 'Disease category' is the classification column

## CNN Model
The model summary for custom CNN model.<br>

![cnn model|300](https://github.com/krazyjoy/AI_CUP/blob/master/images/cnn_model_summary.PNG)

- optimizer: nadam
- minimum lr: 1e-8
- loss: categorical cross entropy
- validation ratio: 5%
- callback: reduce lr on validation loss and early stopping after 10 epochs
- save checkpoint: True

![stft training under 2d cnn model](https://github.com/krazyjoy/AI_CUP/blob/master/images/accuracy_and_loss.PNG)


## DNN Model
The model summary for DNN model.<br>
![dnn model summary](https://github.com/krazyjoy/AI_CUP/blob/master/images/dnn_model_summary.PNG)
- hidden layers: 3
- activation function: sigmoid (hidden nodes), softmax (categorical prediction)
- loss: categorical corss entropy
- optimizer: adam
- metrics: accuracy

## Results UAR and Confusion Matrix
 
|  testing data  |   UAR    | 
|----------------|----------|
|    public      | 0.687    |
|    private     | 0.543    |
<br>

![test public results](https://github.com/krazyjoy/AI_CUP/blob/master/images/test_public.PNG)
![test private results](https://github.com/krazyjoy/AI_CUP/blob/master/images/test_private.PNG)


