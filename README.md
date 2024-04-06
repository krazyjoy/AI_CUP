


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
