# Order 1.1 for Movable Type #

Collect sets of template output to order by a particular datum.


## Installation ##

Unarchive into your Movable Type directory.


## Usage ##

Use the provided template tags to collect and reorder template content. For
example:

    <mt:Order>

        <mt:OrderHeader>
            <div class="site-activity">
        </mt:OrderHeader>

        <mt:Entries>
            <mt:OrderItem>
                <mt:setvarblock name="order_by">
                    <mt:EntryDate utc="1" format="%Y%m%d%H%M%S">
                </mt:setvarblock>
                <mt:Include module="Entry">
            </mt:OrderItem>
        </mt:Entries>

        <mt:Comments>
            <mt:OrderItem>
                <mt:setvarblock name="order_by">
                    <mt:CommentDate utc="1" format="%Y%m%d%H%M%S">
                </mt:setvarblock>
                <mt:Include module="Comment">
            </mt:OrderItem>
        </mt:Comments>
    
        <mt:OrderFooter>
            </div>
        </mt:OrderFooter>

    </mt:Order>


## Template tags ##

The collection and reordering of content is governed through these provided
template tags:


### mt:Order ###

Provides the context in which content items are reordered. All the
`mt:OrderItem` tags contained within an `mt:Order` tag are sorted as one set
of content based on the value of their `order_by` variables.

`mt:Order` takes the following optional attributes:

#### `sort_order` ####

If set to `ascend`, reorders the content from first to last (that is, 1 to 9
and A to Z). Otherwise, the items are sorted in descending order (Z to A and 9
to 1).

#### `offset` ####

Specifies a number of `mt:OrderItem`s to skip. That is, after reordering all
the `mt:OrderItem` tags, discard this many items from the front of the list.

#### `limit` ####

Specifies how many `mt:OrderItem`s to show. That is, after reordering all the
`mt:OrderItem` tags (and discarding items due to `offset`), keep only this
many items from the front of the list.

#### `natural` ####

If set to `1`, sorting values that look like numbers will be sorted
numerically. Otherwise, items are sorted "asciibetically."

When all your sorting values are strings of the same length that you want to
compare strictly character by character, such as timestamps, omit this to save
some computational work. If your content's sorting values are user-provided or
if they're numbers, specify `natural="1"`.

#### `shuffle` ####

If set to `1`, reorder the items randomly instead of using the sorting values.

When using this attribute, you can safely omit the `natural`, `sort_order`,
and `by` attributes, and you need not set sorting values inside your
`mt:OrderItem` tags. `mt:OrderItem pin` attributes are still honored, however.

#### `by` ####

Specifies the name of the variable by which to order items. If not given, the
items are sorted by the values of the default variable `order_by`.

If your `mt:OrderItem` template fragments are already storing their sorting
values in a variable, you can put the name of that variable in the `mt:Order
by` attribute to avoid copying those values into the `order_by` variable.


### `mt:OrderItem` ###

Packages a content fragment for reordering against the sibling `mt:OrderItem`
tags in an `mt:Order`.

Order items are reordered based on the value of the `order_by` variable you
set inside the `mt:OrderItem` tag. Set the `order_by` variable by using the
`mt:SetVarBlock` tag or the `setvar="variable name"` global attribute on a tag
inside the `mt:OrderItem`.

`mt:OrderItem` has one optional attribute:

#### `pin` ####

When specified, instead of being sorted against the other items, the content
of the `mt:OrderItem` will be inserted into the ordered set at the point you
specify.

Pin points are zero-based indexes into the final set. That is, writing
`mt:OrderItem pin="0"` is almost like using the `mt:OrderHeader` tag. Pin
points can also be specified relative to the *end* of the list by using
negative numbers: writing `mt:OrderItem pin="-1"` is almost like using the
`mt:OrderFooter` tag.

If multiple `mt:OrderItem`s have the same `pin` value, those items will be
reordered based on their `order_by` values, then spliced into the full set of
items at the specified `pin` point as a contiguous tranche. Pinned groups are
inserted from left to right (`0` to `-1`), so groups with multiple items may
overlap with groups pinned nearby. For example, if there are four items pinned
at `0` and one item pinned at `1`, in the final reckoning three of the items
pinned at `0` will appear *after* the item at `1`: by the time the item at `1`
is finally pinned, the other items pinned at `0` have already become items #1,
#2, and #3.

Pinned items are put in position *before* the `mt:Order` tag's `limit`
attribute is considered. That is, if you order 11 items, pin one to `-1`
(last), and use `limit="10"` on the `mt:Order` tag, the pinned item will *not*
be shown (it was the eleventh of ten items).


### `mt:OrderHeader` ###

Contains template content that is displayed at the front of the `mt:Order`
loop, as long as there are `mt:OrderItem`s to display.

Content from an `mt:OrderHeader` is shown before the first `mt:OrderItem`, or
even an `mt:OrderItem` pinned to the front with the `pin="0"` attribute.


### `mt:OrderFooter` ###

Contains template content that is displayed at the end of the `mt:Order` loop,
as long as there are `mt:OrderItem`s to display.

Content from an `mt:OrderFooter` is shown after the last `mt:OrderItem`, or
even an `mt:OrderItem` pinned to the end with the `pin="-1"` attribute.


## Changes ##

### 1.1  in development ###

* Added `mt:OrderHeader` and `mt:OrderFooter` tags.
* Added `shuffle` ordering option.
* Added `pin` feature.

### 1.0  30 July 2008 ###

* First release


## License ##

Copyright (c) 2009-2013 Six Apart

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

