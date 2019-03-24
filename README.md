# Formalism

[![Build Status](https://api.cirrus-ci.com/github/AlexWayfer/formalism.svg)](https://cirrus-ci.com/github/AlexWayfer/formalism)

Ruby gem for forms with validations and nesting.

## Usage

```ruby
class FindArtistForm < Formalism::Form
  field :name
  
  private
  
  def validate
    if name.to_s.empty?
      errors.add 'Name is not provided'
    end
  end
  
  def execute
    Artist.first(fields_and_nested_forms)
  end
end

class CreateAlbumForm < Formalism::Form
  field :name, String
  fiels :tags, Array, of: String
  nested :artist, FindArtistForm
  
  private
  
  def validate
    if name.to_s.empty?
      errors.add 'Name is not provided'
    end
  end
  
  def execute
    Album.create(fields_and_nested_forms)
  end
end

form = CreateAlbumForm.new(name: 'Hits', tags: %w[Indie Rock Hits], artist: { name: 'Alex' })
form.run
```
