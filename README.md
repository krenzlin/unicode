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

### error
```python
>>> my_unicode.encode('ascii')
UnicodeEncodeError
```
* also not every byte sequence is valid in utf-8

### error handling
* `.encode('ascii', 'replace')`
 * replace with ?
* `.encode('ascii', 'xmlcharrefreplace')`
 * replace with the html/xml character reference
* `.encode('ascii', 'ignore')`


### implicit conversation
```python
>>> u"Hello" + " world"
u"Hello world"
```
* decodes byte strings 
```python
>>> u"Hello" + " world".decode('ascii')
u"Hello world"
```
* like int to float

* `sys.getdefaultencoding() == 'ascii'`
* thats why `u'Hello' + my_utf8`
 * `UnicodeDecodeError`
 * tries to decode utf-8 as ascii

**works great when everything is ASCII**

* one really odd problem
 * remember: encode is for unicode strings
```python
>>> my_utf8.encode('utf-8')
UnicodeDecodeError: 'ascii' codec can't decode ....
```
* python knows encode is for unicode -> tries to decode the utf-8 byte string to unicode using the default ascii codec
 * this fails

python 3
----
* `str` in python2 was a byte string
* **`str` is now a unicode string**
* if you want a byte string use `b'Hello'`

```python
>>> type(b'Hello')
<class 'bytes'>
```
* Python 3 won't do implicit conversation

```python
>>> 'Hello' + b' world'
TypeError

>>> 'Hello' == b'Hello'
False

>>> d = {'Hello': 'world'}
>>> d[b'Hello']
KeyError
```
* now you have to deal explicitly with both utf-8 and ascii

### files
```python
>>> open('hello.txt', 'r').read()
'Hello world\n'
>>> open('hello.txt', 'rb').read() # opens in binary mode
b'Hello world\n'
```
* but **I/O is always bytes** -> in the first line python encodes the byte string
* so better set the right encoding of the file (on windows default is Windows-1252 not utf-8)
```python
>>> open('hi_utf8.txt', 'r', encoding='utf-8').read()
```
* utf-8 can start with a BOM (byte order mark) which can give an error cause its not a valid character
 * use `utf-8-sig` as encoding (meaning: don't complain about a BOM)
* pitfall: stdin/out/err are **preopened** files with the set encoding depending on how your programm is running
 * could work when piping into a file but breaks when piping into less

### tips
1. unicode sandwich
 * use unicode on the inside
 * decode/encode on the boundaries (bytes outside)
2. know what you have
 * bytes or unicode
 * if byte string you **need** to know the encoding
  * you cannot infer whats the encoding

### get infos on unicode
* `unicodedata` https://docs.python.org/2/library/unicodedata.html
* has normalize function
 * ä into a and dots
```python
>>> print '%r' % unicodedata.normalize('NFD', u'\u00C7') # is a C with cedille
u'C\u0327'
```
