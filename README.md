# AudioAnalyzer

The analyzer is meant to analyze audio data using a logarithmic amplitude as well as a logarithmic shaped point distribution on the frequency-axis by reducing the data to a fix amount of data points per octave (one octave represents a factor 2 in the frequency).

The patch includes 2 methods to analyze audio in different ways. The differences in the audio data between different methods can be seen in a comparison plot using the audioPlot.tox from Greg Hermanovic. https://forum.derivative.ca/t/audio-spectrum-plot/172363

1) A 3-step adaptive-FFT is used to get the frequency response for each of the 3 frequency bands (low, center, high) using various FFT-sizes. This is done to get the best mix between time- and frequency-resolution across the frequency spectrum. You can see the different update rates in the plot when looking closely. The data of the 3 bands gets merged to a complete spectrum (1st output) or it can be smoothed using a 1/x-octave smoothing (2nd output). This results in x 'evenly spaced' data points for every octave (e.g., 12 points = 1/12-octave).

2) The second module includes only a 1/x-octave analyzer. Using just one FFT is has less computation cost but the ‘data quality’ piped into the smoothing is not the same. You can choose between more data points and a low fresh rate (16384 FFT points) at the cost of a slow update rate to get comparable results to the adaptive FFT with an 16k low band. 
Using this method with a small FFT size will result in too few data points per 1/x octave-band – in this case you can lower the number of points per octave to 6 or even to 3.
Check octave_band_analizer/null_band_overview to see the number of data points used for every octave band to balance FFT size, frequency range and 1/x octave smoothing.
The amplitude is using a logarithmic scale to match the human perception of sound. Without that logarithmic shaping the amplitude would be totally over-exposed and contours of the sound over time are masked by this. After this conversion the data is presented in decibel [dB] where every 10dB represents a factor 2 in loudness (-20 dB = ¼ loudness, - 30 dB = 1/8 loudness). Using this logarithmic dB scale lets you analyze and trigger more carefully than the linear scale where the result is overexposed for lower frequencies and relative low amplitude for high frequencies.

Splitting the bands for the adaptive FFT was based on maximizing the range for the high band, because it has the best refresh-rate. To extend this frequency range to lower frequencies the next band is used. Doubling the FFT size for the center band gives 2-times more data points, enough to extend the graph without compromising the update rate too much. The same low-frequency extension is done again with the low-band using the largest FFT window. To get sufficient data points for the lowest octave a 16384 point FFT-window-size is preferable. When this is too slow for your need, you can go to 8192 FFT points and reduce the frequency range on the lower end. This doubles the refresh rate. Also, having a factor 2 between the 3 adaptive FFT frequency bands results in less visible ‘jumps’ where the parts are merged. 
Many Thanks to Elburz Sorkhabi for helping me to come from a smoothing scrip to a node-based approach. Also many thanks to Christian Frechenhäuser and Bileam Tschepe for allowing me to include their samples into the analyzer.

Many thanks for any feedback, ideas or bug reports. I hope you find it useful and can possibly share nice shots of your work. I’ve not spent a lot of time with the output of different visualizers myself because I do not have a project for this right now. 

sven@stevo.io
