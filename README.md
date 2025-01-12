# fluent-plugin-flatten-hash, a plugin for [Fluentd](http://fluentd.org)

[![Build Status](https://travis-ci.org/kazegusuri/fluent-plugin-flatten-hash.svg?branch=master)](https://travis-ci.org/kazegusuri/fluent-plugin-flatten-hash)  

A fluentd plugin to flatten nested hash structure as a flat record with unique keys generated by its path for each values.

# Requirements <a name="requirements"></a>


|fluent-plugin-flatten-hash|fluentd|ruby|
|----|----|----|
|>= 0.5.0 | >= 0.14.8 | >= 2.1 |
| < 0.5.0 | > 0.12.0 | >= 1.9  |

## Installation

Add this line to your application's Gemfile:

    gem 'fluent-plugin-flatten-hash'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install fluent-plugin-flatten-hash

## Configuration

fluent-plugin-flatten-hash supports both Output and Filter plugin.

### Output

You can set a configuration like below:

```
<match message>
  type flatten_hash
  add_tag_prefix flattened.
  separator _
</match>
```

In this configuration, if you get a following nested/complex message:

```js
{
  "message":{
    "today":"good day",
    "tommorow":{
      "is":{
        "a":{
          "bad":"day"
        }
      }
    }
  },
  "days":[
    "2013/08/24",
    "2013/08/25"
  ]
}
```

The message is flattened like below:

```js
{
  "message_today":"good day",
  "message_tommorow_is_a_bad":"day",
  "days_0":"2013/08/24",
  "days_1":"2013/08/25"
}
```

In order to prevent arrays from being indexed, you can use a configuration like below:

```
<match message>
  type flatten_hash
  add_tag_prefix flattened.
  separator _
  flatten_array false
</match>
```

Using the same input, you'll instead end up with a message flattened like below:

```js
{
  "message_today":"good day",
  "message_tommorow_is_a_bad":"day",
  "days":["2013/08/24","2013/08/25"]
}
```

### Filter

You can set a configuration like below:

```
<filter message>
  type flatten_hash
  separator _
</filter>

<match message>
  type stdout
</match>
```

You can also add a base-level prefix to all keys:

```
<filter message>
  type flatten_hash
  separator _
  flatten_array false
  add_prefix "root"
</filter>

<match message>
  type stdout
</match>
```

Using the same input noted above, you'll instead end up with a message flattened like below:

```js
{
  "root_message_today":"good day",
  "root_message_tommorow_is_a_bad":"day",
  "root_days":["2013/08/24","2013/08/25"]
}
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

### Mixins

* HandleTagNameMixin

## Copyright

<table>
  <tr>
    <td>Author</td><td>Masahiro Sano <sabottenda@gmail.com></td>
  </tr>
  <tr>
    <td>Copyright</td><td>Copyright (c) 2013- Masahiro Sano</td>
  </tr>
  <tr>
    <td>License</td><td>MIT License</td>
  </tr>
</table>
