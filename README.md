* notes for `Ned Batchelder: Pragmatic Unicode or How to stop the pain`
 * https://www.youtube.com/watch?v=sgHbC6udIqc

unicode and utf-8
----

## ASCII = one byte to one character encoding
* problem: only 256 characters
* old: use different one byte encodings for different character sets
 * cannot mix
 * no more than 256 neither

## UNICODE = defines characters as integers
* `U+23F0`
* about 1M characters (10% in use)
* but how to map those to the byte world of computers

## UTF-8 = variable length of byte
* ASCII still one byte
* 4 bytes max (till now)
* 

python 2
----
* `str` = sequence of bytes
```python
>>> normal_string = "Hello World!"
>>> type(normal_string)
<type 'str'>
```
* unicode
```python
>>> unicode_string = u"Hello World!"
>>> type(unicode_string)
<type 'unicode'>
```



