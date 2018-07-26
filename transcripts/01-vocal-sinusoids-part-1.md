## Decomposing My Voice

We start with the very first spoken words of this series.

Now in the next two episodes, let's go for a little experiment. We're going to look at a simple analysis-resynthesis technique known as "sinusoidal modeling" and how we can apply it artistically.

For that, we are going to take the `freqpeak~` external from Emmanuel Jourdan's [ZSA Descriptor](http://www.e--j.com/index.php/download-zsa/) package, which is a great library by the way, and for me worked a little better than `[sigmund~]` and all the other alternatives.

What it does is compute trajectories of partials in a spectrum and try to track them over time. The arguments, `2048 4` are simply passed on to the FFT implementation inside, and is of course arbitrary, but a good starting point for a working pitch detection. 

So what do we obtain from it? A list obviously, one of interleaved frequency and amplitude values, to be precise. We can also pass it some attributes, such as the amplitude `[threshold(` below which specifies which partials are going to be ignored, and the amount of `[peaks(` we would like to be returned.

We're going to have to do a little bit list processing here, so first let's de-interlace those values with `[zl delace]` so we get one list of frequencies, and another one of amplitudes.

## Recording Partials Simultaneously with `[poly~]`

Okay, to record the partials in a jitter matrix, let's first save this patch to a tmp folder, and make another one to be used inside of a `[poly~]` object. Instead of letting this degenerate into a copy and paste orgy, we'd like to have everything nice and tidy in one place.

Inside the poly patcher, we'd like to get hold of the voice we're currently observing, and we get that from the first outlet of a `[thispoly~]` object, starting at 1, which is why we immediately subtract 1.

Let's make two inlets for the frequencies and amplitudes lists, respectively. With `[zl nth]` we are able to retrieve the nth value from a list, starting at 1. Here we can use the voice number directly.

We instantiate two `[jit.poke~]` objects for the values, respectively, and fetch our frame count from outside (what's hiding in the `[p start_stop_counter]` subpatch. This goes into the second inlet, as it denotes the x (=time) dimension of the matrix.

Into the first inlet we feed the values for frequency and amplitude, and the last inlet is specified by the voice number, this time starting at 0, as it is the y dimension of the matrix. Let's also make two number boxes so we can observe what is going on, later.

We instantiate the poly with 20 voices and an `@target 0` attribute, which is important, because it allows the lists to be delivered to all voices simultaneously.

We open the tenth voice (arbitrarily) and see that everything seems to work fine.

## Writing Sinusoid Data into Jitter Matrices

Now, let's make those matrices, finally. The first is called 'frequencies' and consists of 1 plane of `float32` values with a maximum length of 1000 (FFT) frames, and 20 possible trajectories. We transform the output a little so we get a nice display in a `[jit.pwindow]`.

We basically copy and paste everything for our amplitudes values, just adapt the display transformations again.

Interestingly, I had to re-initialize the `[send~ fc_rec]` object here to get it to transmit the data into our `[poly~]`, only David Zicarelli knows why ;)

The last thing we want to do is save frequency and amplitude matrix data using `[write(`, so we can resynthesise them in the next episode.

### Main Patcher

```
----------begin_max5_patcher----------
2561.3oc6a08iihiD+4z+Ufxim5NM1FHv9zb5lUZe4z9vo6oUqhb.mzdFBjA
L8Gyps+a+Ja9HPhg3jljr2pok5tSrApp94xU8ykM+wcSltL8UV9Tqex52rlL
4OtaxDUSxFlT88IS2PeMLllqtroaX44z0ro2W1mf8pP09KYbQSqayX4rDAUv
SSVjwBEkRfL2cFA9ww4dKrKdlM7Cx8dK6Y1p+X86U2OOR8LSW9kGH35GZRwF
dRLSnzC7tFSKD0shpkOUD9DOYcGYS.Y64MuqnIdRYiwsDd4yS71VV4cNcZSW
4h2hUsNcprg+7t6j+49wD5Za5nQxz8m4I+wGLTOr5y9yutl9W3hYaegmDk9x
Q7Qb.0xyiPt2B4YOym.iS3g8Pvy0BSHsvDteXxgPlED344TIaINMGKwI6FLC
Q.kvF7fwCfY2qvsy.kRXu.FzA9GRvKhuYCcq0m3IOyxDV1VniNWqwem35Lya
N3uSNBPhFGfrin8ZDsB6NlCGXqK1PEY7WqfwQw8a.fME.U32+g0mdlFa4ZOS
yDQL4DlHZHvfa4rgc9KHvTJLK5lswbQQDK2BYsJNkJHXKDLqzBaqCpbGceHb
.twGBiH2LnpXyRV16ZrYTvftGqRSDIzMkR9elwow0WdNesrQ4zhSEdv6hiiv
dyPxu.YUcOLltT7qngJwa2GjAZRhTurlpFhm14ty4eWccHbih1BJqZYSZjpA
70JtW6AfQJvEFRT9+gAsjojrwZvkfwIrUMrzNjkyMKjkFpDsLY+wmK.DcFAD
RH+EfIv21vDYoxvuVehFJ3OyzNevab3Mh7I6nqqnkURHBANDGcveIMY80KK0
pL12JXIgbCSS4NpAL5lgZ9sZlQequXKi907V3POLFQjcC2HeG0vsq2vTFcFI
OMBRIZruTztMe1M31u5LwS.b8TZbDfA5lrQFoEooxb2138sudFeOyv1Z88b5
BJPM3sbdttLumfwaOv3uixpkwXwULajAZ7Ob3+PhGpGGKqx3prNf3BOlATFx
Ae7VH+jozsaa07jV2hDR9Rp5AM+9ll3IkMQZZJi8Lu99caZklAFk.rnhrR86
U+5vvUrixRJ3JUorQXv4t5GXMN3UQu0VEf0yQ4A332ZxGLPtNNM7qrnVPJ3X
rkkvSZOwtS2QrUzhXwB8r451+dbE61oVdrSltNiGklHUhNXsr4ZwACxtJOb2
1Fi5JRna0byf6EfK8zYNXjE4KoYxghkkdD35NEoowc6p49hYqDUcukmjrGJJ
R21emY70OMv8tLE5byPOaUO4KJRJ6cAL4SrHm9bWzVPiiqlM18w+JMgC4LXB
d4P.1toSVBELzmxCyRii6Xuk87rldh.u3P1K7HwScVChrG3x4aqchl1LJGwW
yxEcaSPWm2skClgBMUrrZV5BACVOIXEcufNUgr8Tx1ws5z9fAu6FDS57.TAZ
2W6HX9s6PWXr9ii2KsffpP263L4d.e4iDPWWP8I0A1qheblfUmf86EvOM9s2
spC3OS9UXMFofmhj.g0mDzr0LXYX8AldmAXZOLXhvkoDI.UD7brBFI5QyqNb
88XqHVrLfod7X9Y.G3iAGkPf8NFSt9F5ac+M2+BnSLiQyeagjvtjW56fk33a
4zC.5L9SNq.PXIc6X6ZLBdEPOkI11r6gttL5BnzA9Uzy6vQuORpGhi1mcPtV
.XPPyTzx5kPrOanrUqgoa1.V9AsCK9m8ZSwltT3d6pZN9vWka3YfbU0paLvO
jF7aG4rXdReYkUFjre8.adZQVXs9VEHzpqYArLD7jFpp+1tDHxKznw0yQIPl
pDnKjRH4ab6gBGCUh4WPcfXnN3bAGMvmfNnGGpZrd0bSkD5iVTt3kETgHiur
PTNMp8xSOIR1vJ9VRiqnP2rDrgXbuiV9c6z2Srl.fdK.sSyd7DbBUOafRd3h
Ki8AziCvMagQ0lg4bsJtX+lo23XlsJtQ.5lYl8TgmLPnLKX4zYBKXI75NFBX
8E4inEGHFgCt6NQBAtFTkq62i75GDKfUf9VLOWnay7v3SXT20HqE4VZsdxOd
PR9g1LNceR9aDOTF4fl81zApLVDUP2OtSXLeaO4zkULqI3x+lFZ8q+Gqe4y+
zi+2bVV9ieIqXIOO7oG+bZXgjUQ9iK9W+5m+4GAOmEaJx4gO98D9xkwr7GVy
EOUr7Qa+G1lkBKcL+A0E7.MI5AZ75zLn+Mv2iXgoI4hrBk0j+nMZANZwKPDT
grrEyn7Uq1waQofeE3tnTPZQDOU1RqKHNMcaWRY.EnDAnrKjELpmjBeqfFyE
u0LmCzz8HWskUVLGXTcOxYqRiiSeoL9rrzFokW1dL3x3qgTJw.7tFL7J3eed
dJAKJR3Iqq1CJmtWfrvO.Xw.WsiKkFY.dKBd3Wy6ZRa4vSILMS5u1joy9vKI
+I9JgdSOOtHqtVTGZMqRy1PSDGQDv7PcsWuWx5GNpdz50pZTn0fABqTtgxcO
haWf1CEg83rGAsRX1dS+wAybu4aTfJEBLMKc6hvzBXRWltsKvdvfq8eRILNI
i7.hzptH0YYT9.llloaZWiOAD+sZiHr8JyS48iMh3GaDwO1HBiqhTYPkStLR
nyu53Npf+9X0+JIS61ecwOkpCQFwpqMzFzHSZz2FJXVAf6OhvoWWN6xyugsB
WIjyovv8Dc6lTw8bVrEp2MrwsW3kblvKYX3sb8On.hDcc7LCcUIjue2+uwn8
PUJFM9UJtDxJSA2yD7KS00QiHlMXbQmwOtX0tjcIBKd0fEuwGVlStXYKvWq8
R.cZ0rsyhCiJ4mitLk12PEyseEy9xnXlVt+K4VN3Z5tub0G2LUyvW6wM2O7v
1ee2chdJDArryn2sVEJi4oq.DjQo.D1lU.BjWvtyxns12Ligqrv3gKO9Nvrc
Wk7aCIyOy2dEiKaUGHgTtSOpyDE530jYuM69phYp5X8t7cNAPN4QIW66dh+0
D9B7ap5GxjCZ9UA9XeOhFp8EiBONGOX46cQyKhfcylo3n3u33p+3AOh0zU23
9EeVS6MPpUwdIkT2rMqXucFusOx3s5t5xTqD01OuUE1c3N52gAzvrezN7Xrf
vFHH7XHIzbCjDpyQYXO5Jmsnwlfl3t52Fdz1TdhHu9M+FOyu5EN0WEsUNUxd
+ugJeOKbvA59lZQ5NNjYN0u7Wxl9f3p2IL.tOrZ+wfUhIhFMB9N6o+8HIxUy
KM3R.mHS7R8G1IEBtEDD.o2HkGb5ZmzNeqwsT227OvG0+C6hFXBlNF9Il3lD
LBxwy7nyiajLSlwg1cJuNa43bkjCFanfzfj3O1rM61qUsOI6OFgT7MwFmOFg
IsMQR1igjLZbC2AhGqrNFK5wmGg7oRNVZHmw.eMYBH1arjzwlGfGiflXShZN
JSCLIeCdLlFL+7luUt7f8N1BRYr2wUXuipvgGSg9OhB6e7D.I+m28+3qgX8O
-----------end_max5_patcher-----------
```
 
### Poly Subpatcher
 
```
----------begin_max5_patcher----------
792.3oc0XssbaCBD8Y6uBM5YEOBP2beqeGc5jAKiSHUFohPoNISx2d4hkkZB
NgLFE65GRDqAN64rKnc8SymEtpdGoML3aA+HX1rmlOal1jxvr8imEtEuqrB2
pmVHqa6JB+kvHy2QWqsVu5tq.I8FkygxpHB8Jf6MtolIX3sD87+Nmhq5mdK8
Fkw3EwCqutS75MnAKJukxt4ZNoTXbYDJStn.HnP8uTy.3h3feNBzM3RMn86t
YqEOzPLahBelxaBB2TUika83U2ReTOO.7f60JdnxPidFrsds1.TM744yU+Ix
QAUhoju1zybq5I3yJRfjhEYxO4vH4yYGdNM1hbw2h0KK6v9wkwLAgeMggWU8
9BoRBWgY2LnfijJuJMo9QZRTA0.PJ7+HQgQ9iTB54ufrS6Zpj3WBhsoUnOgV
ANtVAACoN.yQsDzaDqic3Z5IuMpC8O0gfzEH4mjjH40OW37G3I9aNTbIxboS
Rn2SdIXSoxgsIBw9QDPwljfhbY9e1PBwR3EhTbGUrno9WRs.usohJ5VSZCfV
uSXoUIAcRmKRyOjc.P4erl7koFa3je2QXkziJGEdRNLGSFKDIfytP7XU.Sbq
EVm+tkq476PKPZ5hxjzMdHe.szAlGc9XeleXuIleARbJK.Zg1o94xvQAcXhh
un3ydhtjw.KLNwquC7RgrWYkqnOQRsi2quT2ZEb4hzOhwTlXhIs3VZaSc0C1
p0A5mNBFS8rX8iYEx1BJ93Hth+QeExfrI005NJrTryDTv6fJj4PduKs5n+5v
JJ60+zCZWQY+eEk15NdYO.6e0UvfOHqyQPYXAslMZNf+cR070Dtl1VCDdEYn
cjiOIjybA4zyFxfoPsSc.48QjSBmDGvIyC3fbQIQd.HniBGv2QLniJoEfgSN
vno3zAvkfprgKYuGmCjKri7oEkAPG47olG+pqUNNEOYfhcjQd+DiSHWLEGYb
BYPxjbnwAjguIrZpa.2zbOg2texZHjkMcWs1sxizCoLyPcO7gbx8z94mpsf4
xRdDx5c53lxS1UXZHT+S4yYcz8AVIxOO+uPDC+pg
-----------end_max5_patcher-----------
```