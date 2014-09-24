JointReconstructionCodes
========================

Joint Reconstruction Codes for improved error corection capabilities of fountain code concept

[Fountain codes](http://en.wikipedia.org/wiki/Fountain_code]fountain codes), also known as rateless erasure codes, allow to encode a message as a set of packets, such that any large enough subset is sufficient to reconstruct the message. However, it does not allow for packets to be damaged. This issue can be handled by using some forward error correction(FEC): adding some redundancy, protecting against some noise level. However, the exact damage pattern is stochastic, so some packets are overprotected: have used unnecessarily large amount of redundancy, and some are insufficiently protected – are lost.

Joint reconstruction codes (JRC) are intended to handle this issue. Assuming binary symmetric channel (BSC): that each bit has epsilon independent probability of being flipped (0 <-> 1), the theoretical informational content of such damaged packet is 1 – h(epsilon) bits/position (where h(p) = -p lg(p) – (1-p) lg(1-p) is Shannon entropy). So theoretically we could perform a joint correction/reconstruction of such packets if the sum of "1 – h(epsilon)" exceeds the message size, like in this schematic picture:

https://dl.dropboxusercontent.com/u/12405967/jointsub.png

This implementation uses (unidirectional) correction tree approach ([implementation for BSCand article]( https://indect-project.eu/correction-trees/), [implementation for deletion channel](https://github.com/JarekDuda/DeletionChannelPracticalCorrection)): like sequential decoding for convolutional codes, but for larger state (64 bit here) and specially designed coding. It produces 64 packets (one for each position of the state) of 1/N size: at least N of them is required for reconstruction. The “eps” vector defines the number of received packets (m, chosen in a random way among 64) and damage for each of them (0 - undamaged). Some optimizations and bidirectional correction can be added for improved performance. [Visit here for discussion](http://encode.ru/threads/2056-Enhancing-the-concept-of-fountain-codes-for-better-noise-handling-%28joint-correction%29?p=40598#post40598).

Jarek Duda, September 2014
