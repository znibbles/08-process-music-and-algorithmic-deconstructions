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

### Main Patcher

```
----------begin_max5_patcher----------
2615.3oc6bs0bihiE94jeELd14ktx3FItYu6KY+CrUsO20VtjAYG0M2FP1I8
LUme6qt.X.yEgCPb6IODhsDfN5SmyQGc9j7ec+cK1F8BNcg1+T6KZ2c2ec+c
2IJhWvcYe+tEAnWb8QohaagO4HdYpOwCmr3AY8GQIgn.rn5Xez22hb+1lzXL
1K+NhSvo3PJhRhB2jfcoxVTeo9CxKVhqlrqZ+urmI7PPzApOlJZWXVoxhneO
FKeEKV7f1hc9QH1qL+IiQT2mHg6K0RVlN7F.B4WA1m2ZDOg3Gs8q+N.VH1nD
V+hhS1fCQa8EMIHWPRH48nxEyjZRXtPmWXJ5H1aChRSHaOPwm9TZFPmgzbnz
+.NZWdw4kWVT7iB2W.2mOZT4dSeJJgp5MmCp5MTW.qWwq624.XSUiD5LMW6g
PBMk98L7SV+Ot+9S+Sb8G2+fhpfA3zTzdb9nDE+BUN3EiC0VunQ8GPq5Oco1
XtRn1.LD+yzJWGpI0FS87VtnutXQSZEvKnKydAaOYvM.Sisnv8c2E0AxtH7j
cXacQiUcYYnqV2GbAcegl6dDI70l74bdsUAHq1.nTByRxmCSm9zI+IOveyoT
UPOntTAYkDJsaA9LUwwRUck4wCRYvqOGHsduJ4+vQuSGH1MVKg4.gvFdxmzH
Gba2Ki4a2KSH9Y1f1YNYhi7+9qZ5vMGiHt3k7up8n3yoL0.sGonj8Xpl9.cC
ko.phkpigPkyXcmlpF+T5MxRnenn2Hv6j2n1zLzXw33sYGWV13FcHj1LBo2w
PrkyRn0CZqsEn.ry9ObspCwkaP13VMOF6H93i3jzpQxb2BTbbohq5LI.80Hw
KxovRj0fxhLJJJAejj+7lEkhRXcaJqOeHQJzurx9j+jfHV3IgGHUhUHWjJon
Hz9A5l7+YaJCnaUYeCK1t2Ox8aXux9jDwHPBKGOZkp8v6PG7oa1EERSI+ozw
7onZpT+NjKt0Gtvm4+Ng64J+V1mP7hB4BQEnlWbdy8EMfHNC90ScFwcDhha3
gY5BLXokJSYcxCoaQI7QhLSCXdkznH+pUU7b93czrpiIgg0PQZTb6UlP1+TG
O61HVkAc8tE0jxbnKqcCyvitgOCX06C46mYIV80+BJjDfnXJQND.0KpT5d3o
T2jHe+J8WYMGanFOlRrK9YhG8IQCUVYfc6j3bknEEixdj83TZ0xnn8oUK4Lq
VVQG1lYjtghCXqkhV6FprbsxVjk8YUo7Z9tDdEJr1Za9odBUtM2WF5V4Kr5A
1WxuV4wJ6.urbbNZbleL8RU3FEDvrfO6AHgd3WJlWqTL.4N3uDPqhC+pN8+j
FX4EBmUW0ZqyHJmPzVLi.b8RqpSHb1jBCDSmIXhOy3qZ6b2vyOP63ktJXgAT
DGzpyVM1YXgy.ACv7.F+xmmIkFcIdA5EnLuN0ZBRoQonf3zWaGsfcgVmVVmZ
3FvrLv4rtOfCbcpf8IdOPeVTwLrOsvWXG95WUApZOBIUU.q8xJhUiIByDF+5
EBu0VoW23Kzvj6wmsbHqtwW.7FCeieBkFkLKfLXsCGdyRQbWf7slR7mek0XP
ykyhpLaBaFJaY1mSUC64AkgiJJ+VBrVMOtqVOjfqgqlxnqaZhrSKV0mD11pT
D8Kd8MCooQGRbygk74Mzp1IYK6hRBKV69WJB7keeJMhNTgfqQplP3LkRw.PB
Pk6Kf3EGQBoYiIF.wjJ.aQR1xx3lsvhbRDbtpnZRt4Y2XThmLiUf2WQCztjo
OMRlohRVtExzHEpBOSoP.TUJxsSmFoPUOQfozUzfDBPiBQVg4YUMiaIYVD6f
XoAksq89QaQ9Y4xpX56tR80o7ic4T1vjaJS5FFyK8xIgwZSQR4kad.nY90F2
6.pR6xHx6PZbBy29NseKcIhra2Xx+MLaCSHXbZErK1HblvNdaD9mfi8QtXs+
AXB5z1h0zZ6zUm1dJIYq4Q6sG1sCm7pVTBYOIrHZXUYgqfZ6dU6yfAIUTq6l
ItIbrG+mdH2WGLSZ4FsF1k15FlVsvj17OLRB2E02f35dGDE6OgxaXAU9.+OR
HUIed4zrYzIQjElestXsd0LJ8Bpu.swhc1DbZj+Q7FV28oQ0ew5kNEP0pNYk
2zXf3wsDksNxjDZCJSYKOGqePY6GT19AksWR96biXSijDv8nowdFssQQzKjp
j5yJzWdokJ7F58lMOqqSNRbiB1xV1kl.69r1VTJl6UU3D3xQPUgOc47p1liN
0bFyC7Q01dgI6rRfmJQfIrWTB7dxza0UmTKpiSwZL561hLLxTLepQuXjy0IG
uzmHo4Q4L41c.GwpYr6kX2A6zp7smMsGSz90+CW3pzE0V7u3W90+K+JNzSVh
hYK5JXO9XZNDVHLAyMKDWJnIQh25VSA3nHt.eCJXWOry3nX1HslvLhZcFcGc
StvjHDlfgPwAbNIRPYQC3L2ruXXonnYNkoTWYlVLlR0Xfp1RsSw3sah8aKAv
5fMPuMOy5mTw99e7ypiL02.8tO5Wye9CiYq2iMf5xFNGVBf+JgtQ7nuvi7nz
2T.Lj4BNaqn1BVrBNYIC7FLmXl.Yb6VVebNF9HoXejTrwXEkLeZK8HAAnXsG
IgLaeJa9RvEt3xptK6+DPjs81VYpX5wtRyN16IFlc9Fc.JBglWuPXDC9X+8I
sGOh74qQbd0Bk6nUP+4OC9y3NtrqMObBlMAhmVBF4MUIBApCDTjBfqad2Qbl
otwnhxf2cT9zFOYCJHdISIcxwZYBgAv9AaqaLvdNToscFnFs9sqF8tD7eLkp
z4XclF846vht2P22.f8eDfoIQrYE00dD4RIGwcEgwHvojjxM4+z68DKU8zCd
CbXONOfDVSqqCm0n5xNfcl8GRB3FKlDN7KwKM1rk9D5A15tz.Zh8kkATXG.z
m2.DAYrQa2KK92X9dJMXvcze.G5Rd2FMxLMxFL.5C5Tm9yxfwjRJY1jolRfz
J+Za.ns9eSXkTpYkcf46GWblcVIemvkL+epCLFSIv.mK5Z458pwWq0bebmTV
zZ3DaMwrTZoJKkVFSIM2pd7+lTgPeXGWtIivekHJcJQBU4L1AdkbhPe2Obm4
5NSiT3LLqiY7bb571OgoSzYeU0MFh4UvAL05J3LUBA+MbqWvWoT7yrXUhdtm
eMmgfU7iUtj2FCIoClqureQmUY2H.WYTpkDIs7zSUiXbPCGUS6Q6LpMXLBpW
FifSGFk8KWbNFYXrzZPXjUuXj30UMHUIjU2JLC3N+W3gJ+dEzlywJ+BLT+Gz
fU1Y4sVOeCGT6KP4OkSFqkGjW4gNpwA5AI058I0k7ZcwsjiBMj8HzN1JzNFi
P6XZnPCUCdqNS3z1zNcookoAkQuuP2p7myNaL.tMd4ec4qFiwkK+5pLFMJCR
JzPliQCYrZt5RFpzmlD0NkaYvXqvXnhBug4Xft.UGGAuwVRDElcen45QnOAT
A8JGT3k2RpLahnOM5pHJ0z0PhFsKjABTa2Vxa0Z6xxZ6vxy2cksuyJquqJYs
7Ot++C7OFTgI
-----------end_max5_patcher-----------
```


### Poly Subpatcher
```
----------begin_max5_patcher----------
795.3oc0X0saaBCF8ZxSAhKmRivFSfr61ywzTjCws0cfgAlzjU07rOymgz10
FvQXRUuHfv7QN9b99EdZli2l78rJO2u69SWGmml43.K0rfS60NdYz8IozJvL
OA6w7MO3MWeKIauDVth93wtEE0Y40xTlDdBT6p5kjGJXZ37p32Inodt+p0fB
pL4dt3t0krDo1FD1eg+bWb7plSAvQLdg+omguEPWsitAg5vuRdHE.w6U6Htn
aCgaV64YyZNLebrNMmtcCUb2Exb3Q5i23nvFptDN32KqMjzH6Q5uMANZRL3f
iP.WWsH77Ld002MmbHIkMYrtM5lD1meN95yZtvEegbte1hi.FpSiC5MrNxzv
5Nyn6XaWqdREZqoRYIeSsTWXy4Dqc7Zty5j7rLlPyQO3FfdYIUqJkukcTU4J
t4m8iYvjk5bjflSw8VRLrC9ayERAMSKj+njqvvL8M3U+AU7+BVhTnYO85Atb
QAi86itzrhTtrdKqxE65+wJGd.ka9.wfcpmtuBJHpO4a4nkO7jKe23htvXLd
Sn+v8dhBFtTL4SonDxlEkvHfmnv3gKJEb860JumWUjmd33ElNz3jmaruN.RJ
zRv4HO9SXRCEsNdN2s+PsUIA5SjdY00mTScCh1gJ7WMb+AB5KPCBEyX7cJE6
1j0EozCSkhgPPx+pdyBHwiVwPSthon7wy0.cTidnqVzJTjfdeujfuBJ0KCeb
aI6O0LQBeJm9nMPSWnEQP8JfXz0X7C.ZuTt3++J.v1uY82ppU40kIczt80wb
eg.pg2jbAUxyEuxFr1lOzuYJNwFfyJKfSjA3nlq1Ecxl7xsrxy2jv1.ieOv9
iB3kFB7XU1PC8fnQhCwjPk2R5L91hb0vRs4.gQMS8hZGcHL7zU1V5MZqBUBb
8sc7FAY.1wVvuSLgiA1.o.CPBns0SdMFYqm8hMwMRrf3dxEMXdEx1bzLn04I
V20hLSgQ1PhMoW56AR2CmVTriUV0ZMfgZFnGxAIHZNbIWnuDlZWMY8Ndm8DX
EZoZjEoZdk5R8XE6i0eDDurbkXJp4s5oB4mm8O.0NaXk
-----------end_max5_patcher-----------
```