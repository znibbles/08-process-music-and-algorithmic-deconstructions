## Partial Reassembly

In the previous episode, I left you with a little cliffhanger. We extracted partial trajectories from a voice recording and saved them in Jitter matrices. With that we arrived at a compressed, flexible representation of audio data at the cost of lower fidelity. For a naturalistic rendering our prototype is too crude, but for an artistic playground it provides plenty material. 

Let‘s set out to reassemble those partials and explore  how we can transform them. 

We have saved frequency and amplitude data in files with corresponding suffixes, `_freq.jit` and `_amp.jit`. In the `[p resolve_path]` subpatcher, I have set up a clean way of obtaining the current patcher's pathname and combining it with our file prefix, `01_2d_wavetable`. The `[conformpath]` object converts between file path styles, to prepare our file names for the next step, loading the matrices. The first argument, `max`, specifies the `pathstyle` attribute, which in our case fits best since we do not interact with any third-party tools. The second argument, `boot`, requests that the file path be specified as relative to the current boot volume. This is included here specifically to strip the leading `Macintosh HD:/` part of the file URL, which would lead to trouble later. You can find out more about this awesome object on its reference page.

We then go on and `read` those matrices from the files with the specified root file name, and display them in `[jit.pwindow]`s. What's more, we load the **original** wave file into a `[buffer~]`. We need this to obtain the exact length of it in milliseconds from a `[info~]` object.

## Calculating Playback Speed

In the `[send_frame_count]` subpatcher, we take the reciprocal value of the respective time and multiply it by 1000, to get an appropriate frequency for a `[phasor~]` to loop over our jitter matrices. Simultaneously, we convert milliseconds to samples to multiply the phasor's output, and then divide by the frame size in samples (1024) to get a valid position in our matrices. This is our **frame count**, which we then send to the `[jit.peek~]`s.

Let's construct our `poly~` voice next. We `[receive~]` our frame count and pipe it into the `[jit.peek~]` for each matrix, accompanied by the zero-based voice number. So far, so good. If we leave it at this and wrap it in a `[poly~]` object, we already have a working resynthesis, and it's even pretty intelligible, but it sounds choppy. 

## Bringing in Interpolation

This is because oscillators jump to new frequencies/amplitudes at frame boundaries. We can alleviate that by providing a `[slide~]` object for each of the parameters to interpolate between successive frames. And, while we're at it, we can also expose the `slide_up` and `slide_down` parameters to tweak them from our main patcher.

## Adding Playback Speed Manipulation

Now we already have two parameters to play with and produce interesting effects. Let's create a second one. Because our playback section is essentially just a modulated additive synthesizer, we can easily adjust the playback speed of our sinusoidal model. All we have to do is plug in a multiplication in our phasor's frequency calculation. Here we use a `[live.slider]` to specify it from outside, we just have to take care of retriggering the calculation by hooking it up to the `[info~]`, too. Now we can accelerate, decelerate, reverse, or freeze our data stream. With the interpolation parameters from above those are already a plenty of interesting options to dabble with. Can you come up with some more?