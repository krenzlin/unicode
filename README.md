* notes for `Ned Batchelder: Pragmatic Unicode or How to stop the pain`
 * https://www.youtube.com/watch?v=sgHbC6udIqc

unicode and utf-8
----

### ASCII = one byte to one character encoding
* problem: only 256 characters
* old: use different one byte encodings for different character sets
 * cannot mix
 * no more than 256 neither

### UNICODE = defines characters as integers
* **code points**
* `U+23F0`
* about 1M characters (10% in use)
* but how to map those to the byte world of computers

### UTF-8 = variable length of byte
* ASCII still one byte
* 4 bytes max (till now)

python 2
----
### two datatypes for text
* `str` = sequence of bytes
```python
>>> byte_string = "Hello World!"
>>> type(byte_string)
<type 'str'>
```
* unicode
```python
>>> unicode_string = u"Hello World!"
>>> type(unicode_string)
<type 'unicode'>
```
### to convert
* unicodes have `.encode` -> turns unicode into bytes
* bytes have `.decode` -> turns byte into unicode
* take an argument for the encoding you want (mostly `utf-8`)

#### insert unicode characters
```python
>>> my_unicode = u"Hi \u2219\u01b4"
>>> len(my_unicode) 
5
```
#### convert to byte string
```python
>>> my_byte_string = my_unicode.encode('utf-8')
>>> len(my_byte_string)
8
```
#### and back
```python
>>> my_byte_string.decode('utf-8')
```


