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

## MINT implementation 
of KNN along with a clearer explanation of the algorithm and its implementation.

```mint
// K-Nearest Neighbors (KNN) for MINT
// Uses array a for training data (features)
// Uses array b for training labels
// Uses array c for distances
// Uses array d for sorted indices
// K value is stored in k
// Maximum of 8 training samples, 2 features per sample

:I                     // Initialize KNN system
  [ 0 2 4 6           // Feature 1 values (scaled 0-10)
    2 4 6 8 ] a!      // Feature 2 values
  [ 0 0 1 1           // Training labels (0 or 1)
    0 1 1 0 ] b!      // 8 samples total
  [ 0 0 0 0           // Distance calculation array
    0 0 0 0 ] c!
  [ 0 1 2 3           // Index array for sorting
    4 5 6 7 ] d!      
  3 k!                // Set K to 3
;

:D                     // Calculate distance (x y idx -- dist)
  i!                  // Store index
  y! x!               // Store test point (x,y)
  // Calculate squared differences
  x i 2* a ? - " *    // (x - x1)^2
  y i 2* 1+ a ? - " * // (y - y1)^2
  +                   // Sum squares
;

:S                     // Sort distances using bubble sort
  8 (                // Outer loop
    7 (              // Inner loop
      /i d ? c ? /i 1+ d ? c ? > ( // Compare distances
        /i d ? t!    // Swap indices if needed
        /i 1+ d ? /i d ?!
        t /i 1+ d ?!
      )
    )
  )
;

:V                     // Vote among K nearest neighbors
  0 v! 0 w!          // Clear vote counters
  k @ (              // Loop K times
    /i d ? b ? /T = ( // Check label
      v 1+ v!        // Increment class 0
    ) /E (
      w 1+ w!        // Increment class 1
    )
  )
  v w > /T = .       // Return majority vote
;

:C                     // Classify new point (x y -- label)
  y! x!              // Store test point
  8 (               // For each training sample
    x y /i D        // Calculate distance
    /i c ?!         // Store in distance array
  )
  S                 // Sort distances
  V                 // Vote and return result
;

// Example usage:
// I              // Initialize
// 3 5 C          // Classify point (3,5)
// Expected output: 1 or 0 based on nearest neighbors

// Test multiple points
:T                     // Test points in grid
  5 (                // x coordinates
    5 (             // y coordinates
      /i 2* /j 2* C // Test point (2i,2j)
      ` `          // Space between results
    ) /N           // New line after each row
  )
;

// Display functions
:P                     // Print training data
  `Training Data:` /N
  8 (               // For each sample
    `(` /i 2* a ? .
    `,` /i 2* 1+ a ? .
    `) -> ` /i b ? . /N
  )
;

```


# K-Nearest Neighbors (KNN) Algorithm Explanation

## Basic Concept
K-Nearest Neighbors (KNN) is a simple but powerful classification algorithm that makes predictions based on the similarity between data points. Think of it as classifying something by looking at what category its nearest neighbors belong to.

## How It Works

1. **Data Structure**
   - Each data point has features (like coordinates in space)
   - Each point has a known label (classification)
   - Example: Points might be (x,y) coordinates, labels might be 0 or 1

2. **Classification Process**
   - When given a new point to classify:
     a. Calculate distances to all training points
     b. Find the K nearest neighbors
     c. Take a vote among these neighbors
     d. Assign the majority class to the new point

3. **Distance Calculation**
   - Uses Euclidean distance: sqrt((x2-x1)² + (y2-y1)²)
   - In our implementation, we use squared distance to avoid square root

## Implementation Details

### Key Components

1. **Arrays**
   - `a`: Training data features (x,y coordinates)
   - `b`: Training data labels (0 or 1)
   - `c`: Calculated distances
   - `d`: Sorted indices for nearest neighbor tracking

2. **Key Functions**
   - `D`: Calculate distance between points
   - `S`: Sort distances using bubble sort
   - `V`: Vote among K nearest neighbors
   - `C`: Main classification function

### Process Flow

1. **Initialization** (`:I`)
   - Sets up training data
   - Initializes arrays
   - Sets K value (number of neighbors)

2. **Classification** (`:C`)
   - Takes new point (x,y)
   - Calculates distances to all training points
   - Sorts distances
   - Takes vote among K nearest neighbors
   - Returns predicted class

3. **Testing** (`:T`)
   - Demonstrates classification across a grid
   - Useful for visualizing decision boundaries

## Usage Example

```mint
I               // Initialize system
3 5 C          // Classify point at (3,5)
T              // Test grid of points
P              // Print training data
```

## Limitations and Considerations

1. **Memory Usage**
   - Limited to 8 training samples in this implementation
   - 2 features per sample (x,y coordinates)
   - Fixed K value (3 in this implementation)

2. **Performance**
   - Uses simple bubble sort (O(n²))
   - Integer-based distance calculations
   - No floating-point operations

3. **Accuracy**
   - Depends on training data quality
   - Affected by choice of K
   - Limited by feature scaling

## Applications

KNN can be used for:
- Pattern recognition
- Data classification
- Anomaly detection
- Simple machine learning tasks

This implementation provides a basic but functional KNN classifier suitable for simple classification tasks on a small-scale embedded system.


This implementation provides several advantages for a small system like the TEC:

1. Memory Efficient:
   - Fixed-size arrays
   - Integer-based calculations
   - Minimal variable usage

2. Simple Processing:
   - No complex math (no square roots)
   - Basic sorting algorithm
   - Straightforward voting system

3. Easy to Understand:
   - Clear function separation
   - Simple data structures
   - Demonstrable results

## further work:
1. Add visualization functions?
2. Implement different distance metrics?
3. Add support for more features?
4. Include data preprocessing capabilities?

   
