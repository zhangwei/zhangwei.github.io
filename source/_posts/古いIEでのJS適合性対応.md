title: 古いIEでのJS適合性対応
date: 2015-11-11 18:08:07
tags:
- js

---
- IE7,IE8は問題が多い

​	http://kangax.github.io/compat-table/es5/

- Array.prototype.indexOfのIE7、IE8の退避方法

  ``` javascript
  // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf#Compatibility
  if (!Array.prototype.indexOf) {
    Array.prototype.indexOf = function (searchElement /*, fromIndex */ ) {
      "use strict";

      if (this == null) {
        throw new TypeError();
      }

      var t = Object(this);
      var len = t.length >>> 0;

      if (len === 0) {
        return -1;
      }

      var n = 0;

      if (arguments.length > 0) {
        n = Number(arguments[1]);

        if (n != n) { // shortcut for verifying if it's NaN
        n = 0;
      } else if (n != 0 && n != Infinity && n != -Infinity) {
        n = (n > 0 || -1) * Math.floor(Math.abs(n));
      }
    }

    if (n >= len) {
      return -1;
    }

    var k = n >= 0 ? n : Math.max(len - Math.abs(n), 0);

    for (; k < len; k++) {
      if (k in t && t[k] === searchElement) {
        return k;
      }
    }
    return -1;
  }
  }
  ```

  ​

- Object.keysのIE7、IE8の退避方法

  ``` javascript
  // http://stackoverflow.com/questions/18912932/object-keys-not-working-in-internet-explorer
  if (!Object.keys) {
    Object.keys = function(obj) {
      var keys = [];

      for (var i in obj) {
        if (obj.hasOwnProperty(i)) {
          keys.push(i);
        }
      }

      return keys;
    };
  }
  ```
