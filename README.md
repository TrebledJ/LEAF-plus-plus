# LEAF-plus-plus
Lightweight C++ wrapper for the [LEAF embedded audio framework][leaf].

## Motivation

With C++ in our armament, we may prefer to call the LEAF API in an object-oriented style. Moreover, an OO interface lends itself well to generic programming. For example:

```cpp
template <typename T>
uint16_t get_sample(T& obj) {
    return obj.tick() * A + (A / 2);
}
```

The point is that we can pass any object with `.tick()` (which returns a `float`) and C++ will generate the code for us.


## Usage

Add both `leaf.hpp` and `leaf.cpp` to your project, along with [LEAF's][leaf] `.h` and `.c` files. Don't copy their .hpp and .cpp's into your project.

Most functions can be used in the typical way. Their parameters haven't changed.

One key difference is that we internally define a `LEAF main` object which is used by default.

Here's the [example](https://github.com/spiricom/LEAF#example-of-using-leaf) from the LEAF repo written with the C++ wrapper:

```cpp
// Create your LEAF objects.
leaf::osc::cycle sine;

// Create a memory pool.
#define MEM_SIZE 500000
char memory[MEM_SIZE];

// Create an audio buffer.
#define AUDIO_BUFFER_SIZE 128
float buffer[AUDIO_BUFFER_SIZE];

// Define a RNG.
float random_number() { return 0.4; } // Random number chosen by dice roll-- err-- simulation.

// Initialise the main LEAF object.
leaf::init(48000, AUDIO_BUFFER_SIZE, memory, MEM_SIZE, &randomNumber);

// Initialise your LEAF object.
sine.init(); // Initialised with the main leaf object.

// Set the frequency of the sine oscillator.
sine.setFreq(440.0);

// In the audio callback...
void audioFrame()
{
  for (int i = 0; i < AUDIO_BUFFER_SIZE; i++)
  {
    buffer[i] = sine.tick();
  }
}
```

[leaf]: https://github.com/spiricom/LEAF
