# tec-knn
K-Nearest Neighbors, or KNN is a method to help guess or classify things based on what similar things are like. It's like having a group of friends, and when you need to make a decision, you ask your friends what they think.

Here's how it works:
1. Let's say you have some objects, like fruits, and you want to know if they are apples or oranges. You have a lot of apples and oranges that you already know the answer for.
2. KNN looks at the fruits that are most similar to the one you want to know about. It measures how close they are by looking at features like their color, shape, and size.
3. KNN asks the "K" closest fruits for their opinion. For example, if K is 3, it asks the three fruits that are most similar to the one you want to know about.
4. Based on what the majority of those fruits say, KNN makes a guess. If two out of three similar fruits are oranges, it will guess that the fruit you want to know about is an orange.
5. This way, KNN tries to figure out the type of the unknown fruit by looking at the ones that are most similar to it.

KNN is used in many different areas. For example, it can help computers recognize handwriting, where it looks at the shape of the letters to guess what someone wrote. It can also help recommend movies or books to you based on what you liked before.

Just like when you ask your friends for advice, KNN asks the most similar things to make good guesses or decisions.

```
\\ NEEDS A LOT OF WORK! sudo forth not real forth

variable max-distance   \ A large initial maximum distance
variable max-label      \ Initial maximum label count
variable min-distance   \ Initial minimum distance
variable min-label      \ Initial minimum label count

: euclidean-distance ( p1 p2 -- distance )
  dup - abs           \ Calculate the absolute difference
  dup * swap dup * +  \ Square the differences and sum them up
  sqrt                \ Take the square root of the sum
;

: classify ( sample -- label )
  9999999 max-distance !   \ Set a large initial maximum distance
  0 max-label !           \ Set initial maximum label count
  0 min-distance !        \ Set initial minimum distance
  0 min-label !           \ Set initial minimum label count

  training-data           \ Pointer to the start of training data
  4 8 * swap              \ Total size of the training data (4 samples of 8 bits each)
  0 do
    i 8 * +               \ Calculate the address of the current training sample
    swap euclidean-distance  \ Calculate the distance between the sample and training sample
    min-distance @ <      \ Check if the distance is less than the current minimum distance
    if
      i 8 + min-distance !   \ Update the minimum distance
      i 1+ 2@ min-label !    \ Update the label count of the minimum distance
    then
  loop

  min-label @
;

: setup ( -- )
  \ Setup initialization, if required
;

: loop ( -- )
  \ Test data
  1 0 classify .
  cr
;

: training-data
  0 , 0 ,                 \ Sample 1: Features and label
  0 , 1 ,                 \ Sample 2: Features and label
  1 , 0 ,                 \ Sample 3: Features and label
  1 , 1 ,                 \ Sample 4: Features and label
;

setup
loop

```

how the simplified K-Nearest Neighbors (KNN) code works:

1. The `euclidean-distance` word calculates the Euclidean distance between two data points. It takes two inputs, `p1` and `p2`, and calculates the absolute difference between corresponding elements, squares each difference, sums them up, and finally takes the square root of the sum.

2. The `classify` word performs the KNN classification. It takes a `sample` as input and returns the predicted label.

3. Variables `max-distance`, `max-label`, `min-distance`, and `min-label` are initialized to store distances and labels during classification.

4. The `training-data` is a memory block that stores the feature and label pairs of the training samples. The provided code uses the `create` word to allocate memory and the `,` word to store the feature and label pairs in the memory.

5. The `setup` word is a placeholder for any initialization that may be required before running the main program logic.

6. The `loop` word is the main program logic. It calls the `classify` word with a test data sample and prints the predicted label.

7. Inside the `classify` word, the maximum distance and label count are initially set to large values (`9999999` and `0`, respectively), and the minimum distance and label count are set to initial values (`0`).

8. The `training-data` block is accessed using `training-data` and the total size of the training data is calculated.

9. The KNN classification is performed by iterating over the training samples. For each training sample, the Euclidean distance between the `sample` and the training sample is calculated using `euclidean-distance`.

10. If the calculated distance is smaller than the current minimum distance, the minimum distance and corresponding label count are updated.

11. After iterating over all the training samples, the label with the minimum distance is returned as the predicted label.

12. Finally, in the `loop` word, the test data sample `[1, 0]` is passed to the `classify` word, and the predicted label is printed.

Please note that this explanation is based on the assumption that the code is free from any further errors and the `euclidean-distance` and `classify` words are implemented correctly.
