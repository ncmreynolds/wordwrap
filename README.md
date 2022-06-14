# wordwrap
A word wrap library for Arduino.

This 'greedily' wraps a piece of text to a number of columns, breaking the words at whitespace. It can also constrain text to a 'box' with a fixed maximum number of rows and columns. All text is wrapped by the same code so is effectively constrained at 0xffff rows. Multiple spaces and other whitespace are reduced to a single space.

If text overflows the rows and columns available an elipsis ("...") will be added at the very end to indicate this. If the last box of text overflowed and cut off some of the text, the class variable *overflowed* will be true.

If you pass an uint16_t as the last, optional, argument to the functions, this will be set to the number of lines used by the text after wrapping/boxing.

This is intended for use on microcontrollers with more memory than classic AVR Arduino boards for when displaying text on small TFT displays etc. but will also work on older boards. Obviously it is only truly useful with fixed width fonts on whatever output device you have.

The code is not terribly efficient, with lots of wasteful String use, like others of my libraries it mostly exists to avoid reinventing the wheel in my own projects. Pull requests to improve this are very welcome.

## Usage

Including the library reates and instance of the *wordwrapClass* of *wordwrap* which you will normally use.

To wrap some text into a maximum of eighty columns you do something like...

```c++
String text = "Call me Ishmael. Some years ago—never mind how long precisely—having little or no money in my purse, and nothing particular to interest me on shore, I thought I would sail about a little and see the watery part of the world.";

Serial.println(wordwrap.wrap(text, 80));
```

This will produce output formatted like this...

> Call me Ishmael. Some years ago—never mind how long precisely—having little 
> or no money in my purse, and nothing particular to interest me on shore, I 
> thought I would sail about a little and see the watery part of the world.

To wrap some text into a maximum of five rows and fifty columns you do something like...

```c++
String text = "Call me Ishmael. Some years ago—never mind how long precisely—having little or no money in my purse, and nothing particular to interest me on shore, I thought I would sail about a little and see the watery part of the world.";

Serial.println(wordwrap.box(text, 5, 50));
```

This will produce output formatted like this...

> Call me Ishmael. Some years ago—never mind how 
> long precisely—having little or no money in my 
> purse, and nothing particular to interest me on 
> shore, I thought I would sail about a little and 
> see the watery part of the world.

If the text will not fix in the box and ellipsis are enabled (default) it will render like this...

> Call me Ishmael. Some years ago—never 
> mind how long precisely—having little 
> or no money in my purse, and nothing 
> particular to interest me on shore, I 
> thought I would sail about a little a...

To disable ellipsis, change the value of the class variable *ellipsis*...

```c++
wordwrap.ellipsis = false;
```

By default the library breaks lines with Windows style CR/LF characters. You can disable this by changing the class variable *crlf* to false...

```c++
wordwrap.crlf = false;
```

## Methods

```c++
String wrap(const String& textToConstrain, uint16_t constrainedColumns);
```

```c++
String wrap(const String& textToConstrain, uint16_t constrainedColumns, uint16_t& rowsUsed);
```

```c++
String wrap(const char* textToConstrain, uint16_t constrainedColumns);
```

```c++
String wrap(const char* textToConstrain, uint16_t constrainedColumns, uint16_t& rowsUsed);
```

```c++
String box(const String& textToConstrain, uint16_t constrainedRows, uint16_t constrainedColumns);
```

```c++
String box(const String& textToConstrain, uint16_t constrainedRows, uint16_t constrainedColumns, uint16_t& rowsUsed);
```

```c++
String box(const char* textToConstrain, uint16_t constrainedRows, uint16_t constrainedColumns);
```

```c++
String box(const char* textToConstrain, uint16_t constrainedRows, uint16_t constrainedColumns, uint16_t& rowsUsed);
```
