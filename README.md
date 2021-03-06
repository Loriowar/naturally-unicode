# Naturally-unicode

Natural (version number) sorting with added support for legal document numbering and unicode characters.
See [Sorting for Humans : Natural Sort Order](http://www.codinghorror.com/blog/2007/12/sorting-for-humans-natural-sort-order.html) and [Counting to 10 in Californian](http://www.weblaws.org/blog/2012/08/counting-from-1-to-10-in-californian/)
for the motivations to make this library. This is also the kind of ordering you want if you're sorting version numbers.

The core of the search is [from here](https://github.com/ahoward/version_sorter). I then made
several changes to handle the particular types of numbers that come up in statutes, such
as *335.1, 336, 336a*, etc.

`NaturallyUnicode` will also sort "numbers" in college course code format such as
*MATH101, MATH102, ...*. See the specs for examples.


## Installation

Add this line to your application's Gemfile:

    gem 'naturally-unicode'

And then execute:

    $ bundle

Or install it outside of bundler with:

    $ gem install naturally-unicode


## Usage

```Ruby
require 'naturally-unicode'

# Sort a simple array of strings
NaturallyUnicode.sort(["1.1", "1.10", "1.2"])  # => ["1.1", "1.2", "1.10"]
```

Usually, however, the library is used to sort an array of some kind of
object:


```Ruby
# Sort an array of objects by one attribute
Thing = Struct.new(:number, :name)
objects = [
  Thing.new('1',     'USA'),
  Thing.new('2',     'Canada'),
  Thing.new('1.1',   'Oregon'),
  Thing.new('1.2',   'Washington'),
  Thing.new('1.1.1', 'Portland'),
  Thing.new('1.10',  'Texas'),
  Thing.new('2.1',   'British Columbia'),
  Thing.new('1.3',   'California'),
  Thing.new('1.1.2', 'Eugene')
  ]
objects.sort_by{ |o| NaturallyUnicode.normalize(o.number) }

# Results in:
[<struct Thing number="1.1", name="Oregon">,
 <struct Thing number="1.1.1", name="Portland">,
 <struct Thing number="1.1.2", name="Eugene">,
 <struct Thing number="1.2", name="Washington">,
 <struct Thing number="1.3", name="California">,
 <struct Thing number="1.10", name="Texas">,
 <struct Thing number="2", name="Canada">,
 <struct Thing number="2.1", name="British Columbia">]
```

See [the spec for more examples](https://github.com/Loriowar/naturally-unicode/blob/master/spec/naturally_spec.rb).


## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
