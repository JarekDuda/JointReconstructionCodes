JointReconstructionCodes
========================

Joint Reconstruction Codes for improved error correction capabilities of fountain codes concept

Paper: https://arxiv.org/pdf/1505.07056 

[Fountain codes](https://en.wikipedia.org/wiki/Fountain_code), also known as rateless erasure codes, allow to encode a message as a set of packets, such that any large enough subset is sufficient to reconstruct the message. However, it does not tolerate damaged packets, which have to be discarded. This issue can be handled by using some forward error correction(FEC): adding some redundancy to each packet, protecting against some noise level. However, the exact damage pattern is stochastic, so some packets turn out to be overprotected: have used unnecessarily large amount of redundancy, and some turn out to be insufficiently protected – are lost.

Joint reconstruction codes (JRC) are intended to handle this issue, intuitively by connecting redundancy of all packets. Assuming binary symmetric channel (BSC): that each bit has epsilon independent probability of being flipped (0 <-> 1), the theoretical informational content of such damaged packet is "1 – h(epsilon)" bits/position (where h(p) = -p lg(p) – (1-p) lg(1-p) is Shannon entropy). So theoretically we could perform a joint correction/reconstruction of the message from all received pieces of information (damaged packets), if the sum of "1 – h(epsilon)" exceeds the message size, like in this schematic picture:

https://www.dropbox.com/s/v197uv9z45so8wu/jointsub.png

This implementation uses (unidirectional) correction tree approach ([implementation for BSC and article]( https://arxiv.org/pdf/1204.5317), [implementation for deletion channel](https://github.com/JarekDuda/DeletionChannelPracticalCorrection)): like sequential decoding for convolutional codes, but for larger state (64 bit here) and specially designed coding. Nodes of the tree correspond to reconstruction up to given position: N bits of the message per node. In every step, the most probable leaf of the tree is being found (basing on weights) and expanded.

This implementation produces 64 packets (one for each position of the state) of 1/N size: at least N of them is required for reconstruction. The “eps” vector defines the number of received packets (m, chosen in a random way among 64) and damage for each of them (0 - undamaged). Some optimizations and bidirectional correction can be added for improved performance. [Visit here for discussion](http://encode.su/threads/2056-Enhancing-the-concept-of-fountain-codes-for-better-noise-handling-%28joint-correction%29?p=40598#post40598).

Jarek Duda, September 2014
