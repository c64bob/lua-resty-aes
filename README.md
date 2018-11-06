# lua-resty-aes
AES encryption and decryption for ngx_lua and LuaJIT

Description
===========

This library requires an nginx build with OpenSSL,
the [ngx_lua module](http://wiki.nginx.org/HttpLuaModule), and [LuaJIT 2.0](http://luajit.org/luajit.html).

Synopsis
========

```lua
    # nginx.conf:

    lua_package_path "/path/to/lua-resty-aes/lib/?.lua;;";

    server {
        location = /test {
            content_by_lua_file conf/test.lua;
        }
    }

    -- conf/test.lua:
    local aes = require "resty.aes"
    local str = require "resty.string"
    local aes_128_cbc_md5 = aes:new("AKeyForAES")
        -- the default cipher is AES 128 CBC with 1 round of MD5
        -- for the key and a nil salt
    local encrypted = aes_128_cbc_md5:encrypt("Secret message!")
    ngx.say("AES 128 CBC (MD5) Encrypted HEX: ", str.to_hex(encrypted))
    ngx.say("AES 128 CBC (MD5) Decrypted: ", aes_128_cbc_md5:decrypt(encrypted))

    local aes = require "resty.aes"
    local str = require "resty.string"
    local aes_256_cbc_sha512x5 = aes:new("AKeyForAES-256-CBC",
        "MySalt!!", aes.cipher(256,"cbc"), aes.hash.sha512, 5)
        -- AES 256 CBC with 5 rounds of SHA-512 for the key
        -- and a salt of "MySalt!!"
        -- Note: salt can be either nil or exactly 8 characters long
    local encrypted = aes_256_cbc_sha512x5:encrypt("Really secret message!")
    ngx.say("AES 256 CBC (SHA-512, salted) Encrypted HEX: ", str.to_hex(encrypted))
    ngx.say("AES 256 CBC (SHA-512, salted) Decrypted: ",
        aes_256_cbc_sha512x5:decrypt(encrypted))

    local aes = require "resty.aes"
    local str = require "resty.string"
    local aes_128_cbc_with_iv = assert(aes:new("1234567890123456",
        nil, aes.cipher(128,"cbc"), {iv="1234567890123456"}))
        -- AES 128 CBC with IV and no SALT
    local encrypted = aes_128_cbc_with_iv:encrypt("Really secret message!")
    ngx.say("AES 128 CBC (WITH IV) Encrypted HEX: ", str.to_hex(encrypted))
    ngx.say("AES 128 CBC (WITH IV) Decrypted: ",
        aes_128_cbc_with_iv:decrypt(encrypted))
```



Copyright and License
=====================

This module is licensed under the BSD license.

Copyright (C) 2012-2018, by Yichun "agentzh" Zhang (章亦春) <agentzh@gmail.com>, OpenResty Inc.

All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
