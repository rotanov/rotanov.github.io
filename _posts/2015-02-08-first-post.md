---
layout: post
title:  "Sandbox Playground Post"
date:   2015-02-08 19:45:52
categories: jekyll blog post code
---

First post. It's here as a placeholder for me to play around with jekyll and it's markdown handling. Using `linenos` attribute in `highlight` tag somehow makes everything after `highlight` to become included in `<pre>` tag.

I also would like to use <br />

\`\`\` js <br />
// js code<br />
\`\`\`

syntax.

- one
- two
- three

1. one
2. two
3. three

^ lists wow

{% highlight cpp linenos %}
{% raw %}
#include <iostream>
#include <initializer_list>
#include <cassert>
#include <cmath>

#include "mpi.h"

using namespace std;

template <unsigned rowCount
        , unsigned columnCount
        , typename T = float>
class  Matrix
{
public:
  static const int rowCount_ = rowCount;
  static const int columnCount_ = columnCount;

  T e[columnCount * rowCount];

  Matrix(std::initializer_list<T> l)
  {
    assert(l.size() == rowCount * columnCount);
    for (auto i = l.begin(); i!= l.end(); ++i)
    {
      e[i - l.begin()] = *i;
    }
  }

  Matrix(std::initializer_list<std::initializer_list<T>> l)
  {
    assert(l.size() == rowCount);
    for (auto r = l.begin(); r != l.end(); ++r)
    {
      assert(r->size() == columnCount);
      for (auto c = r->begin(); c != r->end(); ++c)
      {
        e[(r - l.begin()) * columnCount + (c - r->begin())] = *c;
      }
    }
  }

  Matrix()
  {

  }

  void print()
  {
    for (int i = 0; i < rowCount; i++)
    {
      for (int j = 0; j < columnCount; j++)
      {
        cout << e[i * columnCount + j] << " ";
      }
      cout << endl;
    }
  }
};

template <unsigned RC1
        , unsigned CRC12
        , unsigned CC2
        , typename T>
Matrix<RC1, CC2, T> operator *(const Matrix<RC1, CRC12, T>& lhs
                             , const Matrix<CRC12, CC2, T>& rhs)
{
  Matrix<RC1, CC2, T> r;

  for (int i = 0; i < CC2; i++)
  {
    for (int j = 0; j < RC1; j++)
    {
      T s = T();
      for (int k = 0; k < CRC12; k++)
      {
        s += lhs.e[CRC12 * j + k] * rhs.e[CC2 * k + i];
      }
      r.e[j * CC2 + i] = s;
    }
  }

  return r;
}

Matrix<2, 3> a = {{1.0, 2.0, 3.0,},
                  {4.0, 5.0, 6.0,}};

Matrix<3, 6> b = {1.0, 2.0, 3.0, 4.0, 5.0, 6.0,
                  7.0, 8.0, 9.0, 1.0, 2.0, 3.0,
                  4.0, 5.0, 6.0, 7.0, 8.0, 9.0};

int main(int argc, char** argv)
{
  a.print();
  b.print();

  auto c = a * b;
  c.print();
  return 0;
}
{% endraw %}
{% endhighlight %}

{% highlight js linenos %}
function connect() {

// All of sudden a very very long line of comment appears. All of sudden a very very long line of comment appears. All of sudden a very very long line of comment appears. And breaks code layout.

return new Promise(function (resolve, reject) {
  utils.assert(sid_ !== undefined, 'sid is undefined');
  utils.assert(wsURI_ !== undefined, 'wsURI is undefined');

  socket = new WebSocket(wsURI_);

  socket.onmessage = function (event) {
    var data = JSON.parse(event.data);

    if (data.tick !== undefined) {
      if (tickHandler_ !== undefined) {
        tickHandler_(data);
      }

    } else {
      var queue = promiseHandlers[data.action];
      if (queue === undefined || queue.length === 0) {
        // TODO: unexpected error
      }
      var handler = queue.shift();
      if (data.result === 'ok') {
        handler.resolve(data);
      } else {
        utils.logError(event.data);
        handler.reject(data);
      }
    }
  };

  socket.onopen = function (event) {
    resolve();
  };

  socket.onclose = function (event) {
    if (event.wasClean) {
      console.log('Connection closed.');

    } else {
      console.log('Connection is lost.');
    }
    console.log('[Code: ' + event.code + ', reason: ' + event.reason + ']');
    socket = undefined;
  };
});
}
{% endhighlight %}

> This is how `>` quotations get rendered.
> This text actually shouldn't be part of highlighted code block, if it appears so.

Some more text.