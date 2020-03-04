# 解决npm install时出现 “ERROR:root:code for hash md5 was not found.”的问题

## 1. 错误代码：

```
ERROR:root:code for hash md5 was not found.
Traceback (most recent call last):
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 147, in <module>
    globals()[__func_name] = __get_hash(__func_name)
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 97, in __get_builtin_constructor
    raise ValueError('unsupported hash type ' + name)
ValueError: unsupported hash type md5
ERROR:root:code for hash sha1 was not found.
Traceback (most recent call last):
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 147, in <module>
    globals()[__func_name] = __get_hash(__func_name)
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 97, in __get_builtin_constructor
    raise ValueError('unsupported hash type ' + name)
ValueError: unsupported hash type sha1
ERROR:root:code for hash sha224 was not found.
Traceback (most recent call last):
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 147, in <module>
    globals()[__func_name] = __get_hash(__func_name)
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 97, in __get_builtin_constructor
    raise ValueError('unsupported hash type ' + name)
ValueError: unsupported hash type sha224
ERROR:root:code for hash sha256 was not found.
Traceback (most recent call last):
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 147, in <module>
    globals()[__func_name] = __get_hash(__func_name)
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 97, in __get_builtin_constructor
    raise ValueError('unsupported hash type ' + name)
ValueError: unsupported hash type sha256
ERROR:root:code for hash sha384 was not found.
Traceback (most recent call last):
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 147, in <module>
    globals()[__func_name] = __get_hash(__func_name)
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 97, in __get_builtin_constructor
    raise ValueError('unsupported hash type ' + name)
ValueError: unsupported hash type sha384
ERROR:root:code for hash sha512 was not found.
Traceback (most recent call last):
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 147, in <module>
    globals()[__func_name] = __get_hash(__func_name)
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/hashlib.py", line 97, in __get_builtin_constructor
    raise ValueError('unsupported hash type ' + name)
ValueError: unsupported hash type sha512
Traceback (most recent call last):
  File "/usr/local/lib/node_modules/npm/node_modules/node-gyp/gyp/gyp_main.py", line 16, in <module>
    sys.exit(gyp.script_main())
  File "/usr/local/lib/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__init__.py", line 545, in script_main
    return main(sys.argv[1:])
  File "/usr/local/lib/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__init__.py", line 538, in main
    return gyp_main(args)
  File "/usr/local/lib/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__init__.py", line 514, in gyp_main
    options.duplicate_basename_check)
  File "/usr/local/lib/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__init__.py", line 98, in Load
    generator.CalculateVariables(default_variables, params)
  File "/usr/local/lib/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/generator/make.py", line 79, in CalculateVariables
    import gyp.generator.xcode as xcode_generator
  File "/usr/local/lib/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/generator/xcode.py", line 7, in <module>
    import gyp.xcodeproj_file
  File "/usr/local/lib/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/xcodeproj_file.py", line 152, in <module>
    _new_sha1 = hashlib.sha1
AttributeError: 'module' object has no attribute 'sha1'
gyp ERR! configure error 
gyp ERR! stack Error: `gyp` failed with exit code: 1
gyp ERR! stack     at ChildProcess.onCpExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/configure.js:345:16)
gyp ERR! stack     at ChildProcess.emit (events.js:182:13)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:240:12)
gyp ERR! System Darwin 17.7.0
```

## 2. 解决方案：