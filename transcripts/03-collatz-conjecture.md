## Collatz Conjecture

Lately I've come across an interesting problem in mathematics, the _Collatz conjecture_. Let's look it up in [Wikipedia](https://en.wikipedia.org/wiki/Collatz_conjecture):

> The Collatz conjecture is a conjecture in mathematics that concerns a sequence defined as follows: start with any positive integer n. Then each term is obtained from the previous term as follows: if the previous term is even, the next term is one half the previous term. If the previous term is odd, the next term is 3 times the previous term plus 1. The conjecture is that no matter what value of n, the sequence will always reach 1.

More precisely, a recurring cycle of the integers 4, 2, 1.

When I read something about recurring sequences of numbers, I immediately think about sound, or how to make it audible. But first, let's see how we can generate a Collatz sequence in Max.

## Writing the Sequence

Let's start this exploration with a fixed integer of `100`, and a slow `[metro]`, to create a sequence of bangs. Depending on whether the number is even or odd we need to generate the next number in a differently, so let us first check if the current number modulo 2 is equal to 0. Depending on the outcome of this we will trigger a different calculation so let's add a blank `[sel 1 0]`.

Every time this loop runs, we are going to prepare the two calculations: `[/ 2]` if the number is even, and `[* 3] [+ 1]` if it's odd. We are going to store each result in an `[i]` object, but on the cold inlet so it's not immediately output.

The final step is to connect our `[sel]` with the two number containers and feed it into the right inlet of our current number, which will be the base of calculations in the next step. Now let's start the metro and take a look at the results, inserting some debug break points.

Now that we're done, let's extract an abstraction from that. That reminds me, we should probably crack that open again and add a second inlet, to be able to supply a different starting number.

## Arpeggiating

My initial idea was to sonify this sequence in the fashion of an arpeggiator, however numbers of a few thousands are clearly impracticable for this. That's why we are going to devise a new abstraction called `voice_freq` with a modulo operation first, thus limiting the range of numbers to a maximum of this first argument. Then we offset it by a second argument to send it into an `[mtof]` to derive MIDI pitches from it.

Once we're ready, let's instantiate this object and give it two arguments for offset and modulo operator. Using a `[multislider]`, let's visualize the output of `[collatz_sequence]` with an inverse point scroll.

## Polyphony

Now let's duplicate this combination a few times and change all the values to produce an interesting polyrhythmic pattern. Connecting everything to the `[multislider]`, we can already see those emerge.

Let's connect the frequency outlets to an `[mc.voiceallocator~]` with 24 channels. We connect that to an `[mc.target]` and also supply an envelope here, which we will trigger whenever the allocator assigns a new value. It is assigned the same voice as an `[mc.cycle~]` which we instantiate here.

We apply the envelope using an `[mc.*~]`, and use an `[mc.meter~]` to visualize it. Then we downmix to stereo and output it to our audio device.

Let's restart everything and listen to the melodic patterns emerging.

## Max Representations

### Main Patcher

```
----------begin_max5_patcher----------
1732.3ocya00TaiCE8Y3WgG+3NzLRW889z9+nSGFiQPcWGapsCKscJ+12qjc
nDPIQNQICvz7grvGeO5nyUWY2ec4E42z9jsOO6uy9b1EW7qKu3BeStFtX56W
jur3ox5hde2xWVtv9yaKJeN+pwi1rZYUSscveX5TiOTLT90pl6utyVNLd5oJ
5BMA+gy0.HMfVcUFSZVHcMJnbilSYDyUYbwBx3qYe4OfztZXMJjoVqt0eI0d
y29DWk6Z62Wdo6kqhOXpqdzt39hplWBnGK5ZJVZ2dGBFwscU1lghgp1l8PD.
ejGHXvK3ZkQi7fRuf64AISKLFlQdUFkI8DgZqDAepULfPXJZZr0ksqZ7.Aqu
v7ce3GOXGgOe4p5gpod2WceSQc9UY4t+cWcawf6C0U8343KAnYY9KAVGRRC1
tqsME2TaecP2W7n81qKFF5ptY0f8OepeZrXZvvw00qrs2st40s+5S+5q7oA8
MN1plpg9geLhNOPGVhiTti8IEYQnSPcay86bv9MmsBuZRF7b0+01tgsbxF6s
Wct9sYJUar+GN.rl8GrOMrFldDba6yY+SwpgVGbYzfJUXlRRbB36lZR4NdLC
fsJIoyP1ETgINnIxtQfYaIATc.KoPwsdxlhP4L7.TN1QkZhzLRgvH.tXtTx3
7sfr.OOoZj+54jHInjQWJtDDLfvYBzd5jqHXokKpqZr3rEGv8XXNWhIHIvDp
IQCCznb.3LbxBJu1M0vluGcXJBRKEU9ix5igiBId.L+O34MBmRMbC0khCjmZ
0CMsTyPQ281gzHZfImFtVP0TMQiMJL6gP1Zd8d6fOcpSkT0rEeExgvFXp6Ab
YMyzcMXLSMl.RCtOl4yWDbSQy8ACTJrqEpPN.R3tUMk9U2kBZ.XlEr2RCTF0
wCx8udusjIYsEwU6fXL7cRLSGq31aenEEQSkF3s3GeAM7QsIUpIRElyTaHJ2
73oiQI3mjLE2Pl9g59CEZoTp78.cFWHMJtRaLTgQJAoqGTCgqjqe02SIir.D
.kPzbBiIw2VeQj8kO.yoCZx4VxtuMAEiXzpCN0SoYlzZv8XaUosnttsrXns6
4s49OyBbnloBb.CVhigvzTm6u3PYlcxH5DxHOT7undy+aPdfucdPvlx3oPkN
2H0NoNUOY5CJo.Wo.57iRc8glELLAnRHA30CWeWm86nyT1e7UiUMHvoD5QVv
nMbFCb0QHDiTCvvk.HI.EcGLpzxBrCqRh99h6suiFP+JxbcHjBXLzMJv43gi
4tUENwGTm8fPHcMxDoMzSY4Cks00EC+75d62WYaJsykEBK.BwB9rGGFMrUu.
QJcGsCcsYRPi0ZiqE3Q67qzNHYnLiSFvBFP9fZjdYh4P4hsl6mIOM9BB9A3K
vTSqKBs.MXFAJQdd7E.SJ8EPGbwbEAbsbLzU.WY.tlHNS9BjOP9BgE.mKeAZ
x8EP+8C2WHHYb17EfSiu.WmQUy99DX3SqTTiArVnXmKeAQJ8EvxVHIYWBNG9
Bf7CjuPXAvYxW.TI2WPPOBegfjw4xW.NnBo7aLWec0s1tcN2u+OWDjcrfo0U
Spz.gBn2na6SnrwhnXTMn0Z7cjAjpIOBzAkKHFvW24zTJhBGGvhu.9dJ0ru5
m1ctWKatKrXT716.0FTXTa2BdRPBZ5tKMscKBBYQR2niWWHGIUk0eN7koIc8
Zy2V1cSu7QNPLZhlwMayPBLKDILxolOP1xAG9OStxzz6JSA1QXKGhLB5Je32
eis6J+Ztv2gb2sz5MOEG9KZW6aRP8sq5JWCw58CNi9BN2Z6GpZd4wX3yubaC
19ttFKRtmcfLx9PRsQmVVsw9Py3ueWy4fOwGU4cc17aLE3+llN92sw84UAKN
5PRDSHISA4whAoMubZ6bofcZry.z7vPSNNngXflsgB9sRFbcQl2cm2ziVTFw
3zXFfUbojLCrmiQ7SmwjduSF5OzwNHSiMbOZjHwfDj.jXlXPhlBjzw5jsUwh
KS+6GYAuffIBIjbGK0pdefrOJ6M7ZhlqGEG5gldRfdeQMENE1LLUDPC7TnQi
IGHSjBjhI0jOvYImNiBZ1IQ+xikeoGK+FSVw0Z5iCoXRBxRhCJMVMCjbMSLP
CmFOORr76wpY.y4Ry.wXlBpTfjJVMCM0ZlnfFNIqIFjwxuGslQb1zLzyFRwj
oOEhSpIVjN1gIZzSCR9pWhB52L3lnoAzXFIMuegSi6ZQwCO7nsqep2dPxWV7
sV+El9J+WqZF+peaRx6rOV0+5+GVjWzU90pAa4vptwm69mjiarY9xVLDaVUM
EkX3gP52bH2yne+CEiQheOjt72W9+z94nYC
-----------end_max5_patcher-----------
```

### collatz_sequence

```
----------begin_max5_patcher----------
732.3ocwWtsaaCCCF9ZmmBAAra177DkrNMf9jLLL3lHz5hD6.amtLTz28oC1
8vpSiciRVtHAhRV+5ilhj4gEI3qq2aZwnui9AJI4gEIIdSNCI8iSvaJ1ubcQ
qeY3xp0lNbZXlpca7i8SQ5Mtsna4skU27qFyxtvVCBRFw8gkKnTBAD5TTt0V
Jh8z2ne97tVuqaXagdqASc+YqIrmX7SOP4J+Qq9569JLbzVVuYioxqOdvVY0
JimMha7iKV39JchfWY9sUfgspyrOr2ki5KnG1WPkpLUvWPzLEWnfTDkRy.uQ
InYTshkaMpy3toluqozA9Xdmb7+SxAVF0AIMmAJglSjWNxYQj7uffn7VGTPF
yaD.fIn4b4YgcZDY+yHVbXmoxzgfAolyYTJ6rvNDQ1+FhFkX9KE6jHxdqYMB
PjQ4m8NY9IhPleh8hctfSntPbP0emWv0RJGTtxA4tBAuC+rCw+0EU2fSe92Q
cF5H5Kt5pC3HnyzQ.RYeB.BGjboBn1Zgri3HlcffJhr+o4eGXbzuL2Ajwrfm
kjY+dm66uAHxLomVBjKXBs1RKiG6Wz7OBsenN53WrF53msN5BGfQIGlG4LhJ
SxDLpJDmS.9DbDjQnULaZ8aNdcY0+1Gu+r6r+ZWPa8tlkCt7PlQa74SGwUl1
txphtx5pWrFaiioHKGi5kmiPjiIDKBBolfP5HnibB5nhfN7InyqOK0MqLM9L
SmcgcgrioL67qLLtxjyuxjwUFNIkgoHMOBwSC2lOpPOmVXS4ps01xN84XnJU
eiKu7Owly8ku4zLo19QMjgj+9yADvMmzuB+fSkO1Iymspr7M+U0.C.WeP9Fc
t3yGcJwn4A.OMkfonDMFwjjoVc3jYZ5AGuoLaw1s2aZZ6WrWBaOE2U6u8qR8
CKqBC88PfaL2WNr9fkhFaKEc19I103OV38hbb3Qqs4Qp1U1mJwBmURe+JUEa
LsaKBb3aqYwiK9qTARFU
-----------end_max5_patcher-----------
```

### voice_freq

```
----------begin_max5_patcher----------
373.3ocqTF1SCBCDF9ys+JZ5heRz.E1h5eEiwzAUsKiVRoLwrr+6B2M1zIlw
X7ktbu658dOsWYKkvWZqUkb1SrmYDxVJg.RsBj8wDdtrNcsrDRiaTeZWthGf
+kWU6A4bu8sNQSUt1rV4gMDsWrP5S+Pad+UmJ0iFNew8gALA9Sr.BZVYubrN
1J+oEBk7eUnvpv4G1fNC5kl96t47VocTZ6Rv0w1srYQ8xl3brE8n35XSa78h
WxDh2MrYhQhm.u5VL03EOF7vpOtgvXAdQE1s9OjD92d8vYWpMOWYvyzNMsIS
A8b3H.B5+d4I7r7jLHbtr2TQWLoPU3q0lS+FCz3s5+F+RakKsqGvmwAriMel
pzqMRu1Z9QNBLmdOfGpOICvm4SfOwCvmjIvmnA3S7e7AutjEEaTtx8ICVzLW
tx5ZCeH.B0FLDFf3N0FcW9nhz0LW5aFJqbPawqWjvwsZyTNSkFxk1BWikvLu
QlqJKjHGvSC5N52.t2kfDA
-----------end_max5_patcher-----------
```

