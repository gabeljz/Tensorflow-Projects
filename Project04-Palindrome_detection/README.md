# 1. Palindrome Detection

This problem is chosen to demostrate if there is any advantages of Bidirectional LSTM over Unidirectional LSTM.

The idea is that the same LSTM layer is run on the sequence in the original and the reverse directiions. 

Since a palindrome is identical in both directions, then the two outputs of the LSTM layer on both directions should also be the same.

Then it will be a simple operation to compare the two LSTM output to detect if the sequence is a palindrome.

# 2. Merge mode of the BiDirectional layer

The outputs from LSTM in both directions should be the same.

The two outputs are then merged in the BiDirectional layer, and the result is fed into a Dense layer.

The different merge mode can be considered using this example both outputs:

```
#!python

output = [2,5,3,7]
```

## 2.1 Concatenation merge mode

```
#!python

output1 = [2,5,3,7]
output2 = [2,5,3,7]
merged_result = output1 + output2
print(merged_result)
# result is [2, 5, 3, 7, 2, 5, 3, 7]
```

Assuming that the label of a palindrome is **1**, while a non-palindrome is **0**.

The Dense layer will then have to be trained with this merged result as its input, to produce the output **1**.

A single layer of Dense layer might not be sufficient for this function.

## 2.2 Average merge mode

```
#!python

output1 = [2,5,3,7]
output2 = [2,5,3,7]
merged_result = [x1 - x2 for x1, x2 in zip(output1, output2)]
print(merged_result)
# [0, 0, 0, 0]
```

Assuming that the label of a palindrome is **1**, while a non-palindrome is **0**.

The Dense layer will then have to be trained with this merged result as its input, to produce the output **1**.

A single layer of Dense layer might not be sufficient for this function.

## 2.3 Difference merge mode

Imagine that there is a difference merge mode.

```
#!python

output1 = [2,5,3,7]
output2 = [2,5,3,7]
merged_result = [x1 - x2 for x1, x2 in zip(output1, output2)]
print(merged_result)
# merged_result is [2.0, 5.0, 3.0, 7.0]
```

Assuming that the label of a palindrome is **1**, while a non-palindrome is **0**.

The Dense layer will then have to be trained with this merged result as its input, to produce the output **1**.

A single layer of Dense layer might not be sufficient for this function.

But, we can use the **Dense layer to simulate a difference merge mode**.

If we invert the labels, such that the label of a palindrome is **0**, while a non-palindrome is **1**.

And we use the "concat" merge mode or the "None" merge mode.

Then the Dense layer can be trained easily to find the difference of the merged result to produce a **0**.

```
#!python

output1 = [2,5,3,7]
output2 = [2,5,3,7]
merged_result = output1 + output2
print(merged_result)
# result is [2, 5, 3, 7, 2, 5, 3, 7]

def Dense_function(inputs):
    bias = 0;
    weights = [1, 1, 1, 1, -1, -1, -1, -1]
    weight_inputs = [x*w for x,w in zip(inputs, weights)]
    return sum(weight_inputs) + bias

output = Dense_function(merged_result)
# output is 0
```

# 3. Training Results

|LSTM size|Uni (x2)| Bi_Con| Bi_Ave| Bi_Con_Inv|
|:--------|-------:|------:|------:|----------:|
|16       |  0.9949| 0.9995| 0.9971|     0.9992|
|8        |  0.9889| 0.9787| 0.9943|     0.9897|
|4        |  0.9780|#0.9909| 0.9655|     0.9619|
|4 @ 2000e|  0.9697|#0.8623| 0.9775|     0.9717|
|2        |  0.9263| 0.9299| 0.8409|     0.9528|
|1        |  0.8418| 0.8820| 0.8682|     0.8753|

**Note**: The actual size of the LSTM output for the Unidirectional model is **twice** of that of the Bidirectional models.

The validated accuracy of the Bidirectional concat model with LSTM(4) is very inconsistent, and it seems to be very susceptible to being "struck" with either extremely good accuracy or anomally bad accuracy.

## 3.1. Conclusion

The accuracies of the have a rather wide range between different training.

The differences in accuracies of the different models are too small to make a conclusion.

# 4. Other Observations

In a set of random sequence, the ratio of palindromes to the total number of sequences is:

$$\begin{aligned}
& \frac{num^{\thinspace len \thinspace / \thinspace 2}}{num^{\thinspace len}}
= \frac{1}{num^{\thinspace len \thinspace / \thinspace 2}}
\\
\\ &\text{where}
\\ & num \rightarrow \text{number of possible characters}
\\ & len \rightarrow \text{length of the sequence}
\end{aligned}$$

So in a set of sequences with $10$ characters, only $8.417 \times 10^{-8}$ of them should be palindrome.

In this notebook, when a model has low accuracy, most of the wrong detections seem to be **false positives**.

This problem might be caused by half of the training data used for the training being palindrome.

Hence the model has a bias to label a sequence as a palindrome.


# 5. Further Work

1. From the training results, it seems like the Dense layer is doing the bulk of the detection. Hence it will be good to compare with a model that only has Dense layers.
2. Build a GAN to generate and detect palindrome.