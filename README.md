# ArduinoRingBuffer

This is a simple ring buffer library for the Arduino. It is written in vanilla C, and can easily be modified to work with other platforms simply be removing the `#include "Arduino.h" `.  It can buffer any fixed size object (ints, floats, structs, etc...).

## Project History
I needed a way to buffer sensor events for an engineering IOT project for a class I was taking at Cornell. I couldn't find any suitable Arduino Libraries that could buffer any sized object, so I wrote my own.

I decided to give object oriented programming a shot using only C (no C++) with this library, of course, it still works with C++. Using C structs and function pointers, the library creates RingBuf objects that are complete with their own methods and attributes. Note that every method (except constructor), takes a `RingBuf *self` pointer. This is the equivalent of the `this` pointer in C++, but the C++ compiler automatically passes it behind the scenes. For this library, you must manually pass this as the first argument.

## Use Cases

A ring buffer is used when passing asynchronous io between two threads. In the case of the Arduino, it is very useful for buffering data in an interrupt routine that is later processed in your `void loop()`.

## Install

Download this file as a zip, and extract the resulting folder into your Arduino Libraries folder. [Installing an Arduino Library] (https://www.arduino.cc/en/Guide/Libraries).

## Examples

Look at the examples folder for several examples.

## Contributing

If you find this Arduino library helpful, click the Star button, and you will make my day.

Feel free to improve this library. Fork it, make your changes, then submit a pull request!

## API


### Constructor

```
RingBuf *RingBuf_new(int size, int len);
```

Creates a new RingBuf object of len elements that are size bytes each. A pointer to the new RingBuf object is returned on success. On failure (lack of memory), a null pointer is returned.
This would be the equivalent of `new RingBuf(int size, int len)` in C++.

### Deconstructor

```
int RingBuf_delete(RingBuf *self);
```

Deletes the RingBuf, and frees up all the memory associated with it.

## Methods


### add()

```
int add(RingBuf *self, void *object);
```

Append an element to the buffer, where object is a pointer to object you wish to append. Returns -1 on a full buffer. On success, returns the position (index) in the buffer where the element was added.

### peek()

```
void *peek(RingBuf *self, unsigned int num);
```

Peek at the num'th element in the buffer. Returns a void pointer to the location of the num'th element. If num is out of bounds or the num'th element is empty, a NULL pointer is returned. Cast the result of this call into a pointer of whatever type you are storing in the buffer. Note that this gives you direct memory access to the location of the num'th element in the buffer, allowing you to directly edit elements in the buffer.

### pull()

```
void *pull(RingBuf *self, void *object);
```

Pull the first element out of the buffer. The first element is copied into the location pointed to by object. Returns a NULL pointer if the buffer is empty, otherwise returns object.

### numElements()
```
unsigned int numElements(RingBuf *self);
```

Returns number of elements in buffer.

### isFull()
```
bool isFull(RingBuf *self);
```

Returns true if buffer is full, otherwise false.


### isEmpty()

```
bool isEmpty(RingBuf *self);
```

Returns true if buffer is empty, false otherwise.

## License

This library is open-source, and licensed under the [MIT license](http://opensource.org/licenses/MIT). Do whatever you like with it, but contributions are appreciated.
