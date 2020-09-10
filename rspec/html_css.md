# Better CSS tests
- [x] have_color
- [x] have_background_color
- [x] have_border_color
- [x] have_same_width_as
- [x] have_same_height_as
- [x] be_next_to
- [x] be_above
- [x] be_below
- [ ] be_directly_above
- [x] have_child
## Color
`.style("color")`
vs
`native.style("color")`

`{"color"=>"rgba(0, 0, 238, 1)"}`
vs
`"rgba(0, 0, 238, 1)"`

#### Hope
`element.style("color").name # => "Orange"`
or
```
color = Color.parse(element.style("color") )
color.name # => "Orange" 
```

#### EVEN BETTER
```ruby
expect(navbar).to have_background_color("blue")
expect(navbar).to have_color("white")
expect(navbar).to have_border_color("blue")
```

## Position, Height, Width
`el.evaluate_script("this.getBoundingClientRect().left;")`
vs
`el.rect.left`

`rect` doesn’t actually have the `top`, `bottom`, `left`, `right` methods :/

Instead try 
```ruby
top = el.rect.y.round
bottom = el.rect.height + top

left = el.rect.x.round
right = left + el.rect.width
```

#### hope
```ruby
expect(nav).to have_same_width_as(container)

expect(nav).to have_same_height_as(footer)

expect(nav).to be_next_to(div)

expect(label).to be_above(input)

expect(label).to be_directly_above(input)

```

### Next to other element
```ruby

top_el = el.rect.top
bottom_el = el.rect.bottom 
        
other_top = other_el.rect.top 
        expect(top_el..bottom_el).to cover(other_top)

```

Probably want to round, b/c this is annoying
```bash
Expected the top of 'menu.jpg' to be between 1600.015625 and 1659.015625, but was 1600 instead.
```


## Label matches Input
```ruby
 
    address_label = find("label", :text => /Zip Code/i)
    for_attribute = address_label[:for]

    if for_attribute.empty?
      expect(for_attribute).to_not be_empty,
        "Expected label’s for attribute to be set to a non empty value, was '#{for_attribute}' instead."
    else
      all_inputs = all("input")
  
      all_input_ids = all_inputs.map { |input| input[:id] }
  
      expect(all_input_ids.count(for_attribute)).to eq(1),
        "Expected label’s for attribute(#{for_attribute}) to match only 1 of the ids of an <input> tag (#{all_input_ids}), but found 0 or more than 1."

```

Can do the same thing with other attributes like `expect(link[:target]).to eq "_blank"`.


## Get Parent Element
`element.find(:xpath, "..")`
vs
`element.parent`

## Assert child element
```ruby
expect(page).to have_css("nav a", :text => /Sign In/i),
      "Expected page to have an a tag with text ‘Sign In' inside a nav, but didn’t find one."

```

Better
```ruby
expect("nav").to have_child("a")

# implementation
# el.tag_name + child_arg
# = have_css("tag child", opts)
```

## Find `img` with `src` that contains `...`
```ruby
app_store_image = find("img[src*='app-store-badge.png'")

```


## Resize page
```ruby
page.driver.browser.manage.window.resize_to(1024, 768)

current_window.resize_to(1024, 768)
```


# Better Rspec comparisons
```ruby
expect(5).to be_between(1, 10)
expect(5).to be_within(0.05).of value
```
