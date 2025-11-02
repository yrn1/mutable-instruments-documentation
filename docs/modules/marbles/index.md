![](images/front_small.jpg)

## Key data

*Random sampler*

Parameter    | Value
-------------|------
Width        | 18HP
Depth        | 25mm
+12V current | 80mA
-12V current | 20mA
Lifetime     | 03/18 to 04/22
Modulargrid  | [Link](https://www.modulargrid.net/e/mutable-instruments-marbles)
Processor    | STM32F405RGT6 @ 168 MHz
DAC          | DAC8164

## Features

### Random gate generator

#### Master clock

* Internal clock with adjustable rate (with V/O CV input), or division/multiplication of an external clock.
* Range selection button further multiplying or dividing the clock by 4.
* Rhythm follower/predictor to lock onto uneven clock divisions or rhythmic patterns.
* Adjustable jitter (knob and CV), going from perfect tracking to completely erroneous – but always preserving the overall tempo.

#### Two-channel random rhythm generator

* Three gate outputs: *t<sub>2</sub>* is the main output carrying the jittery clock, *t<sub>1</sub>* and *t<sub>3</sub>* are the complementary random rhythm output.
* Three generative models, with CV-controlled bias parameter increasing the density of notes on one channel or the other:
	1. Random routing of each clock pulse to either outputs, following a coin toss.
	2. Selection of a random division factor for one output, and the reciprocal factor for the other.
	3. Generation of random kick/snare patterns using a process similar to [Grids](../grids).
* Adjustable gate duration, from short triggers to full length. Gate duration can be randomized.

### Random voltage generator

* 3 outputs, either clocked by the 3 outputs of the random gate generator, or by a common external clock.
* Distribution control: **SPREAD** control, scanning between constant, bell-shaped, uniform or discrete distributions; and **BIAS** control biasing the generated voltage towards the bottom or top of the voltage range.
* Adjustable range: 0 to +2V (for melodies), 0 to +5V, -5V to +5V.


#### Quantized or smooth... CV Post-processor

* The **STEPS** parameter controls the steppiness/quantization of the output voltages.
* Turn this knob clockwise and a progressive quantizer is applied to the voltages - progressively reducing the probability of hitting a note outside of the scale, then making accidentals less likely, then giving more weight to the root and fifth - and at the extreme yielding only octaves.
* If steppy is not your thing, turn counterclockwise to increasingly slew the output voltages to the point that the module produces smooth, continuous curves.

#### Programmable quantizer

* 6 programmable scales.
* Scales are programmed by playing a short jam in the target scale: **Marbles** learns which notes are more prominent than others.

#### Output diversity

* The three outputs can all follow the settings dialed on the control panel, or react in different and opposite ways. The turn of a knob can completely push your patch towards a new direction!

### Random looping and shuffling

* **DEJA VU** parameter increasing the probably of re-playing past material to the point that the generated output forms a loop... then increasing the probability of randomizing the order of this loop.
* The **DEJA VU** control applies to the random rhythm, the random voltages, or to both, or neither of them.
* Adjustable loop length from 1 to 16 steps.

### External CV processing

* An external CV can be recorded in the **DEJA VU** loop in place of internal random voltages.
* All transformations performed by the random voltage generator (looping, shuffling, spreading, transposition, quantization, lag-processing) can be performed on external voltages.
* TLDR: live remixing of external sequences!

### Specifications

* Analog random source.
* All inputs: 100k impedance, DC to 3.2kHz.
* Maximum input clock rate: 1kHz for the ***t*** Section, 8kHz for the ***X*** section.
* 32kHz refresh rate.
* 14-bit DAC with accurate software calibration - error below 1mV.
* 12-bit CV capture.
* Output levels: -5V to +5V for CVs  (largest setting), 0V to +8V for gates.
* Input CV range: -5V to +5V.
* Front panel with Computer Modern labels, just like on your calculus textbook.

## Installation

Marbles requires a **-12V/+12V** power supply (2x5 pin connector). The red stripe of the ribbon cable (-12V side) must be oriented on the same side as the “Red stripe” marking on the module and on your power distribution board.
The module draws **80mA** from the +12V rail, and **20mA** from the -12V rail.

## Marbles' recipe for random music

1. Start with a **clock** – generated internally or divided/multiplied from an external clock signal.
2. If required, add some **jitter** to it, from slight humanization to complete chaos.
3. Split this randomized clock into two streams of random triggers to generate two **contrasting rhythmic patterns** complementing the main clock.
4. Generate three **random voltages** in sync with the rhythmic patterns obtained at the previous step.
5. Transform the random voltages to **spread them further apart, or concentrate them** around a specific voltage.
6. Add a pinch of **lag-processing** to obtain smooth random modulations... or **quantization** to get random tunes.

Steps **1 to 3** are handled by the left half of the module, the random rhythms being generated on the outputs labelled **t**. In your Eurorack system such duties might have been performed by modules like Grids and Branches.

Steps **4 to 6** are handled by the right half of the module, the random voltages being generated on the outputs labelled **X**. A large number of modules would be necessary to patch this functionality: a triple noise source and sample&hold, waveshapers, quantizers, and lag processors.

And now let’s take it further: what if everything the module did could be controlled by a slowly evolving or lockable loop, like with Music Thing’s Turing Machine? That’s what the **DEJA VU** section is for.

Time to dive into the details!

## *t* Generator

![](images/manual.png)

The **t generator** produces random gates by generating a jittery master clock (which is output on ***t<sub>2</sub>***) and deriving from it two streams of random gates which are output on ***t<sub>1</sub>*** and ***t<sub>3</sub>***.

**A. Clock rate.** 120 BPM at 12 o’clock.

**B. Clock range.** Divides or multiplies the clock rate by 4.

**C. Amount of randomness in the clock timing** - perfectly stable, then simulating an instrumentalist lagging and catching up, then... complete chaos.

**D. Bias.** Controls whether gates are more likely to occur on ***t<sub>1</sub>*** or ***t<sub>3</sub>***. Several methods are available for splitting the master clock into ***t<sub>1</sub>*** and ***t<sub>3</sub>***, selected by the button **[E]**:

  1. A coin is tossed at every pulse of ***t<sub>2</sub>***, to decide whether the pulse is passed to ***t<sub>1</sub>*** or ***t<sub>3</sub>***. **BIAS** controls the fairness of the coin toss.
  2. ***t<sub>1</sub>*** and ***t<sub>3</sub>*** are generated by respectively multiplying and dividing ***t<sub>2</sub>*** by a random ratio. Turn the **BIAS** knob fully clockwise or counter-clockwise to reach more extreme ratios.
  3. the triggers alternate between ***t<sub>1</sub>*** and ***t<sub>3</sub>***, following the same kind of regularity as kick/snare drum patterns.

**1. BIAS, RATE** (with V/O scaling) and **JITTER** CV inputs.

**2. External clock input.** The clock signal patched in this input replaces the internal clock. In this case, the **RATE** knob and CV input are re-purposed as a division/multiplication control, and the jitter setting is applied to the external clock.

**3. Gate outputs.**

Hold the button **[E]** and turn **BIAS** to adjust the gate length from 1% to 99%, or **JITTER** to adjust the gate length randomization (from deterministic to completely random). Only ***t<sub>1</sub>*** and ***t<sub>3</sub>*** are affected by this. ***t<sub>2</sub>*** has a constant 50% duty cycle.

![](images/gate_length_jitter.jpg)

## DEJA VU section

Whenever the module needs to make a random choice (for instance, to decide on the amount of jitter to apply on the next tick of its clock, or to generate a random voltage for one of its outputs), it queries the **DEJA VU** section. The **DEJA VU** section either recyles a previously generated random choice, or samples fresh random data from a hardware random source.

**F. G.** These buttons control whether the **DEJA VU** settings apply to the **t or X** section (or neither, or both). For example, the module can generate a non-repeating sequence of voltages locked to a looping rhythm (**t** enabled, **X** disabled); or cycle through the same sequence of voltages on an ever-changing rhythm (**t** disabled, **X** enabled).

**H. Probability of re-cycling random decisions/voltages** from the past:

* From 7 o’clock to 12 o’clock, this probability goes from 0 (completely random) to 1 (locked loop).
* At 12 o’clock, the module is thus stuck in a loop, because it never generates fresh random data. In this case, the illuminated pushbuttons [F] and [G] blink.
* From 12 o’clock to 5 o’clock, the probability of randomly jumping within the loop goes from 0 to 1.
* At 5 o’clock, the module thus plays random permutations of the same set of decisions/voltages.

**I. Loop length.** Lengths of 5, 7, 10 and 14 can be obtained by setting the knob between the graduations printed on the panel.

**4. DEJA VU CV input.**

## *X* Generator

The X generator generates **three independent random** voltages output on ***X<sub>1</sub>***, ***X<sub>2</sub>*** and ***X<sub>3</sub>***. They are clocked by the three outputs from the **t** section, or by a common external clock.

**J. Output voltage range.** 0 to +2V, 0 to +5V or -5 to +5V.

**K. Probability distribution** width and shape. Turning counter-clockwise from 12 o’clock, the voltages are increasingly concentrated near the center of the range. Fully counter-clockwise, a constant voltage is output. At 12 o’clock, they follow a bell curve - more likely to occur near the center but able to reach the extremes. At 2 o’clock, they occupy the entire voltage range with equal probability. Past this point, extreme values become more likely. Fully clockwise, only the minimum and maximum voltages are possible, turning ***X<sub>1</sub>***, ***X<sub>2</sub>*** and ***X<sub>3</sub>*** into random gates.

**L. Distribution bias.** Skews the distribution towards low or high voltages. Think of this as the probabilistic equivalent of an offset: it does not shift the voltage down or up, but biases the decision towards the bottom or top of the voltage range.

In the illustration below, the pink histogram represents the distribution of possible output voltages: the tallest bar corresponds to the most likely outcome. The teal oscillogram is an example of
output voltage sequence.

![](images/distributions.jpg)

**M. Horizontal and vertical “steppiness”** of the generated voltages. At 12 o’clock, generates the typical S&H steps. Turn CCW to generate smoother edges, then random linear segments, then smooth random curves. Turn CW to quantize the generated voltages to a scale, then to progressively strip the scale of its notes until only the root note remains.

**N. Controls how the outputs** ***X<sub>1</sub>***, ***X<sub>2</sub>*** **and** ***X<sub>3</sub>*** **react** to the settings dialed on the knobs **[K]**, **[L]** and **[M]** – allowing you to obtain different flavors of random voltages from the 3 outputs. Diversity is fun!

![](images/x_mode_1.png) All channels follow the settings on the control panel.

![](images/x_mode_2.png) ***X<sub>2</sub>*** follows the control panel, while ***X<sub>1</sub>*** and ***X<sub>3</sub>*** take opposite values. For example, if STEPS is turned fully CW, ***X<sub>1</sub>*** and ***X<sub>3</sub>*** will be smooth while ***X<sub>2</sub>*** is quantized to the root note and its octaves.

![](images/x_mode_3.png) ***X<sub>3</sub>*** follows the control panel, ***X<sub>1</sub>*** reacts in the opposite direction, and ***X<sub>2</sub>*** always stays in the middle (steppy, unbiased, bell-curve).

**O. External processing mode**.

**5. STEPS, SPREAD** and **BIAS** CV inputs.

**6. External clock input.** When patched, all three outputs follow the same external clock, as opposed to being clocked by the three outputs from the t section.

**7. CV outputs.**

## Sampling external CVs with the *X* generator

Press the button **[O]** to enable the external processing mode. In this mode, the module samples the voltage present on the **SPREAD CV** input **(5)** whenever a random value is needed for one of the **X** outputs.

A couple of interesting notes about external CV processing:

* **BIAS** **[L]** acts as a transposition control, shifting voltages up and down, and **SPREAD** **[K]** controls the range of this transposition.
* When no patch cable is inserted in the ***X*** section's clock input **(6)**, the three **X** output will contain the same melody, but with some notes frozen/sustained on outputs 1 and 3 – because each output is sampled at its own pace.
* When the channel diversity setting **[N]** is set to the first (green) position, and the module is externally clocked, and an external CV is fed to the module, you'd expect all 3 outputs to carry the same voltage, right? That's right, but that would be boring... In this particular configuration, the module switches to a shift-register mode in which ***X<sub>2</sub>*** carries ***X<sub>1</sub>***'s voltage shifted by one clock tick, and ***X<sub>3</sub>*** carries ***X<sub>2</sub>***'s voltage shifted by one clock tick.
* All outputs follow the value of the **STEPS** **[M]** knob. Always. Irrespectively of the channel diversity setting **[N]**.
* Because some sequencers do not exactly change their output CVs at the same time as they send their gate signals, **Marbles** tolerates up to 3ms of difference between the transitions.

## *Y* Generator

The Y generator, by default, is a **smooth, full-range (-5V to +5V), random source** that is clocked at **1/16** the rate of ***X<sub>2</sub>***. These settings can be modified by holding the button **[N]** and adjusting **RATE** (division factor relative to ***X<sub>2</sub>***, from **1/64** to **1**), **SPREAD**, **BIAS** and **STEPS** while the button is held. The ***Y*** output is never affected by the **DEJA VU** settings.

![](images/y_output_settings.jpg)

## STEPS quantizer

The **STEPS** knob progressively eliminates notes from a chromatic scale: first to reveal an interesting scale, then to mask all notes except the most salient ones in this scale. The example below is for a C-major scale (first factory preset).

![](images/scale.png)

### Selecting a scale

Hold the voltage range button **[J]** for 2 seconds and repeatedly press it to select a scale. The color of the blinking LED and the rate of blinking indicates the active scale. Six memory slots are available for recording scales. They are pre-programmed with scales rooted in C (0V).

![](images/preset_scales.png)

### Programming a scale

1. Connect the CV and Gate outputs of a keyboard or MIDI interface to the **SPREAD (5)** and **CLOCK (6)** inputs respectively.
2. Hold the external processing mode button **[O]** for 2 seconds. The LED above the scale selection button **[J]** blinks and indicates the active scale. This scale is going to be reprogrammed!
3. Play a little jam in the scale you want to program. Fifty notes, or more, is the recommended length.
4. Press the button **[O]** when done.

The module analyzes your jam to measure how frequently each note occurs. The least frequently played notes will be the first to be eliminated when **STEPS** is turned clockwise from 12 o’clock. The most frequently played note will be the last one to remain when **STEPS** is at 5 o’clock.

**Note:** it is also possible, at step 3, to play the scale in ascending order, instead of a long melody. In this case, the module will not know the relative importance of each note of the scale, and the gradual scale “carving” will not be performed: turning the **STEPS** button from 12 o’clock to 5’clock will not modify the scale.

One can take advantage of the way the module counts notes to program several subsets of the same scale along the course of the **STEPS** knob. For example if we play :

**C D E F G A B C D F G A C F G C G C**

We'll get, right after 12 o'clock, **C D E F G A B** ; then **C D F G A** (least frequent notes E and B are eliminated) ; then **C F G** (less frequent notes D and A eliminated) ; then **C G** (F is the next to be eliminated) ; then **C** (which is the most frequent note in the fragment).

### Resetting a scale to a factory default

1. Hold the external processing mode button **[O]** for 2 seconds to enter the scale recording mode. The LED above the scale selection button **[J]** blinks and indicates the active scale. This scale is going to be reprogrammed!
2. Hold the external processing mode button **[O]** for 2 seconds. The currently active scale is reprogrammed to its factory data.

## Tips and tricks

* If **DEJA VU** is past 12 o’clock and the loop length is set to 1, the outputs remain frozen in the same state.
* If **DEJA VU** is around 11 o’clock, the loop will slowly mutate.
* The **DEJA VU** knob has a “virtual notch” at 12 o’clock - even if it is not exactly at 12 o’clock, you will still get a perfectly non-random loop.
* Once a sequence is looping, it is still possible to alter it with **SPREAD/BIAS** to map it to a different range of voltages.
* Once a sequence is looping, a rapid double press on the **DEJA VU** buttons reseeds it.
* When the X section is not externally clocked, ***X<sub>1</sub>***, ***X<sub>2</sub>*** and ***X<sub>3</sub>*** are rhythmically independent from each other, each output changing its voltage at its own pace. Setting the loop **LENGTH** to 3 (for example), will cause each output to go through a 3-note sequence independently from the other, creating polyrhythmic effects.
* Self-patching is a rewarding technique with Marbles! In particular, the ***Y*** output provides a useful slow modulation source for randomizing the other parameters of the module.

* * *

## Advanced topics

### <a name="firmware"></a> Firmware update procedure

Unplug all CV inputs/outputs from the module. Connect the output of your audio interface/sound card to the **RATE** CV input. Set the **RATE** knob **(A)** to an intermediate position. Power on your modular system with the **t** section **DEJA VU** illuminated push-button **(F)** pressed.

Make sure that no additional sound (such as email notification sounds, background music etc.) from your computer will be played during the procedure. Make sure that your speakers/monitors are not connected to your audio interface - the noises emitted during the procedure are aggressive and can harm your hearing. On non-studio audio equipment (for example the line output from a desktop computer), you might have to turn up the gain to the maximum.

When you are all set, play the firmware update file into the module. While the module receives data, the **RATE** LED will act as vu-meter (lit in orange when the signal level is optimal). Try adjusting the **RATE** knob to adjust gain. When the end of the audio file is reached, the module automatically restarts - if it is not the case, please retry the procedure from the beginning.

In case the signal level is inadequate, all LEDs will blink in red. Press the button **(F)** and retry with a higher gain. If this does not help, please retry the procedure from another computer/audio interface, and make sure that no piece of equipment or software effect (equalizer, automatic gain control, FX processor) is inserted in the signal chain.

* * *

## Common issues

### The output range selection LED is off

(... and the three **X** output do not seem to work).

This is normal behavior when external processing mode **O** is enabled.

In external processing mode, there is no point defining what would be an output voltage range, since the output voltage is related to whatever you decide to feed into the module input! As a result, the output voltage range goes off.

## The sequence does not have the expected **LENGTH**

A common misunderstanding is that **LENGTH** adjusts a global loop length – that would cause everything in the module to reset itself after *N* clock ticks.

However, setting the loop length to 3 does not mean that you will create a looping 3-beat pattern. It means that each "decision" (internally, sampling a value from the internal random source) will cycle over 3 values:

  > When the X section is not externally clocked, ***X1***, ***X2*** and ***X3*** are rhythmically independent from each other, each output changing its voltage at its own pace. Setting the loop **LENGTH** to 3 (for example), will cause each output to go through a 3-note sequence independently from the other, creating polyrhythmic effects.

Let's take an example... If the t section is in green mode a "decision" is taken at each pulse of the clock, and is used to select between t1 and t3. So if they repeat with a length of 3, you could get a pattern like this:

    t1: x--x--x--x--x--x--
    t3: -xx-xx-xx-xx-xx-xx

X1 will pick a voltage at each pulse of t1, and will repeat these decisions (A, F, G in the example below) with a cycle length of 3. X3 will pick a voltage at each pulse of t3, and will repeat the same 3 decisions (C, F, E in the example below). This will result in the following pattern:

    t1: A--F--G--A--F--G--
    t3: -CF-EC-FE-CF-EC-FE

As you can see, it does not loop over 3 beats, it loops over 9.

If you want a more structured "global" loop length of 3, you can self-patch t2 to the clock input of the X section. Then all the X outputs will all change at the same "master" rate:

    t1: AFGAFGAFG
    t3: CFECFECFE

And you can use an external S&H on t1/X1 and t3/X3 to hold the voltages:

    t1: A--A--A--
    t3: -FE-FE-FE

## Alternate modes

A long press on the mode button for the *t* generator (labelled **E** in the [manual](../manual/)) activates a variant of the currently selected mode, represented by a blinking LED.

* Green blinking: **Independent Bernoulli**. The choice between ***t<sub>1</sub>*** and ***t<sub>3</sub>*** is no longer exclusive. The coin toss is independent, and both channels, or none of them can be on.
* Orange blinking: **Deterministic divider/multiplier**. A clock division or multiplication ratio selected by **BIAS** is applied to ***t<sub>3</sub>***, and its reciprocal is applied to ***t<sub>1</sub>***. There is no randomness.
* Red blinking: **Three states**. The module randomly selects between a trigger on ***t<sub>1</sub>***, on ***t<sub>3</sub>***, or on no output at all.

A short press on the button reverts to the normal mode.

## Commented out "markov" mode

The code contains an unaccessible, additional mode for the *t* generator.

[Implementation here](https://github.com/pichenettes/eurorack/blob/master/marbles/random/t_generator.cc#L213
)

Just a bunch of heuristics to make “balanced” rhythmic patterns.

To implement a true Markov model, you’d need a large table storing the probability of playing a hit at clock tick t, for all the possible decisions taken in the past n ticks (aka “contexts”). You need long contexts if you want to learn well large-scale structures (like, a whole bar), with the drawback that you’d need a lot of training data to estimate the probabilities, a lot of memory to store everything, and then you might overfit and learn some patterns by heart. Not so great.

A different idea to drastically reduce the number of parameters of the model is to state that the probability of playing a hit at clock tick t is only influenced by several “features” extracted from the context – the code and comments are clear about what these features are.

The weight of each of these features is empirically chosen, and is influenced by the **BIAS** knob. I didn’t spend a lot of time tweaking these weights. One could actually train this properly, it’s just a little logistic regression. But you’d still have to introduce some very arbitrary choices in how the weights are modulated by the BIAS knob.

## Revisions

### v1.3 (final)

#### Fixes and minor improvements

* Fixed a bug causing spurious jumps in the **DEJA VU** loop playback position (occurring randomly, when **DEJA VU** is turned CCW), and another bug causing an unexpected change in the loop (occurring randomly, on average once every 37 hours) when **DEJA VU** is locked at 12 o'clock. Thanks to Tyler/[float32](https://github.com/float32) for finding these two!
* Fixed a bug occurring when a buggy external clock source "stutters" by sending two rapid rising edges distant by a few ms at each clock tick. 
* Lowered the amount of hysteresis in the quantizers responsible for converting a voltage or knob position into discrete settings (eg: a clock divider ratio, or **LENGTH**). This makes the relationship between a voltage (or knob position) and a specific setting more consistent.
* Increased the width of the "virtual notch" for the 14-step setting of the **LENGTH** knob.

#### Implicit and explicit reset

* If a clock pulse arrives after a long pause (3 seconds, or 4 times the interval between two clock ticks, whichever the longest), the module will reset all counters, dividers, patterns, and the step number of the relevant section (t or X). Provided **DEJA VU** is enabled and set at 12 o'clock, this gives a completely reproducible behavior at each restart of the clock.
* Hold the t and X section mode buttons (labelled **E** and **N** in the manual) simultaneously to repurpose the **JITTER** and **STEPS** CV inputs as explicit reset inputs for the **t** and **X** sections. The corresponding LEDs blink temporarily for 2s when the repurposed reset inputs are enabled, and go off when they are disabled (which is the default setting).

The **JITTER CV** input is repurposed as a reset input for the t section: a rising edge on this input will reset, **on the next clock tick**, the clock divider, the step counter in the **DEJA VU** loop and the position of the rhythmic pattern.

The **STEPS CV** input is repurposed as a reset input for the X section: a rising edge on this input will reset, **on the next clock tick**, the position of the **DEJA VU** loop.

If a reset is received on the **t** side, this reset will be cascaded to the **X** section in the two following situations:
* The **X** section's clock input is left unpatched (ie, X1, X2, X3 are internally clocked by t1, t2, t3 respectively).
* The **X** section's clock is directly patched from t1, t2 or t3 (self-patching detection).

Outside of these cases, the **X** section is considered to be running independently from the **t** section, and a reset on the **t** side won't be propagated to the **X** side.

Emphasis was put on **on the next clock tick**: for this reset feature to work accurately, the reset pulse must precede, ideally by 1ms, the subsequent clock pulse. If the reset arrives right at the same time as the first clock pulse, the behavior is unspecified! Don't hesitate to delay your clock (or advance your reset) if you experience odd timing.

### v1.2

* When the module is in external processing mode, with an external clock for the **X** section, and when this clock's patch cable is unplugged from the **X** clock input, so that an internal clock is now used for the **X** section, the acquired CV data on output **X1** (which was shifted to outputs **X2** and **X3**) is now copied to the **DEJA VU** loop of all three outputs.

* If a 3-second pause (or a pause lasting more than 4 clock ticks, whichever is the longest) is observed on the **t** section's clock signal, the rhythmic pattern played by the **t** section will reset to the first step at the next clock tick.

* New *super lock* feature. A long press on the **t** or **X** **DEJA VU** buttons locks the random generation for this section – which will stop responding to the **DEJA VU** knob. When a section is in this *super locked* state, the illuminated push-button blinks rapidly. Press it to bring it back to normal. In other words, the **t** and **X** section can be in these 3 states:
  * Illuminated push-button off: random without repetition (equivalent to **DEJA VU** set to the minimum).
  * Illuminated push-button on: randomness amount controlled by the **DEJA VU** knob.
  * Illuminated push-button rapidly blinking: locked loop without randomness (equivalent to **DEJA VU** locked to 12 o’clock).

### v1.1

* Powering the module on while pressing the **X** mode selection button **(N)** enables (or disables) the color-blind mode. LED brightness and blinking patterns are more contrasted. Glowing, moderate brightness for the first setting, high intensity for the middle setting, low brightness for the last setting. When selecting scales, the number of blinks (from 1 to 6 – continuous) indicates the selected scale.

* Fixed a bug causing the X's section clock follower to temporarily go haywire when a patch cable is inserted in the clock input of the X section.

### v1.0

Initial release.
