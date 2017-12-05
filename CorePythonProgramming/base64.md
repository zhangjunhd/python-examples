# base64编码
base64要求把每三个8bit的字节转换为四个6bit的字节(`3*8=4*6`)，然后把6bit的字节再添两位高位0，组成四个8bit字节，也就是说，转换后的字符串理论上将要比原来的长1/3。

## 1.base64编码
```python
# -*- coding: utf-8 -*-
import base64

initial_data = open(__file__, 'rt').read()

encoded_data = base64.b64encode(initial_data)

print '%d bytes before encoding' % len(initial_data)
print '%d bytes after encoding' % len(encoded_data)
print
print encoded_data
```

输出：

    260 bytes before encoding
    348 bytes after encoding

    IyAtKi0gY29kaW5nOiB1dGYtOCAtKi0KaW1wb3J0IGJhc2U2NAoKaW5pdGlhbF9kYXRhID0gb3BlbihfX2ZpbGVfXywgJ3J0JykucmVhZCgpCgplbmNvZGVkX2RhdGEgPSBiYXNlNjQuYjY0ZW5jb2RlKGluaXRpYWxfZGF0YSkKCnByaW50ICclZCBieXRlcyBiZWZvcmUgZW5jb2RpbmcnICUgbGVuKGluaXRpYWxfZGF0YSkKcHJpbnQgJyVkIGJ5dGVzIGFmdGVyIGVuY29kaW5nJyAlIGxlbihlbmNvZGVkX2RhdGEpCnByaW50CnByaW50IGVuY29kZWRfZGF0YQo=

## 2.base64解码
```python
# -*- coding: utf-8 -*-
import base64

original_string = 'This is the data, in the clear.'
print 'Original:', original_string

encoded_string = base64.b64encode(original_string)
print 'Encoded:', encoded_string

decoded_string = base64.b64decode(encoded_string)
print 'Decoded:', decoded_string
```

输出：

    Original: This is the data, in the clear.
    Encoded: VGhpcyBpcyB0aGUgZGF0YSwgaW4gdGhlIGNsZWFyLg==
    Decoded: This is the data, in the clear.

## 3.URL_Safe变体
默认的base64字母表可能会使用+和/，而这些字符可能出现在URL中，因此必须为这些字符指定可选择的编码情况，+由符号-来代替，而/由下划线_来代替，其他字母表不变。

```python
# -*- coding: utf-8 -*-
import base64

for original in ['\xfb\xef', '\xff\xff']:
    print 'Original         :', repr(original)
    print 'Standard encoding:', base64.standard_b64encode(original)
    print 'URL-safe encoding:', base64.urlsafe_b64encode(original)
    print
```

输出：

    Original         : '\xfb\xef'
    Standard encoding: ++8=
    URL-safe encoding: --8=

    Original         : '\xff\xff'
    Standard encoding: //8=
    URL-safe encoding: __8=
