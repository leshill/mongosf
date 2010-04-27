!SLIDE

# Extras

!SLIDE

# Dirty Attributes

!SLIDE

# Dirty Attributes: Anything changed?

    @@@ ruby
    person = Person.new(:first_name => "Jake")
    person.first_name = "Toruk"
    person.changed? # =>  true
    person.first_name_changed? # => true

!SLIDE

# Dirty Attributes: Tracking changes

    @@@ ruby
    person.changed? # =>  true
    person.first_name_was # => "Jake"
    person.first_name_change #  => [ "Jake", "Toruk" ]
    person.changes # => { "first_name" => [ "Jake", "Toruk" ] }

!SLIDE

# Versioning

    @@@ ruby
    class Person
      include Mongoid::Document
      include Mongoid::Versioning
    end

    person.first_name # => "Jake"
    person.update_attributes(:first_name => "Toruk")

    person.first_name # => "Toruk"
    person.versions.last.first_name # => "Jake"

!SLIDE

# Timestamps

    @@@ ruby
    class Person
      include Mongoid::Document
      include Mongoid::Timestamps
    end

    person = Person.create(:first_name => "Jake")
    person.created_at # Fri Apr 30 19:09:31 UTC 2010
    person.updated_at # Fri Apr 30 19:09:31 UTC 2010

    person.update_attributes(:first_name => "Toruk")
    person.created_at # Fri Apr 30 19:09:31 UTC 2010
    person.updated_at # Fri Apr 30 19:09:33 UTC 2010

!SLIDE

# Keys

    @@@ ruby
    class Person
      include Mongoid::Document

      field :first_name
      field :last_name

      key :first_name, :last_name
    end

    person = Person.new(:first_name => "Grace", :last_name => "Augustine")
    Person.find("grace-augustine")

!SLIDE

# Indexes

    @@@ ruby
    class Person
      include Mongoid::Document

      index [[ :first_name, Mongo::ASCENDING ],
             [ :last_name, Mongo::ASCENDING ]], :unique => true
    end

!SLIDE

# Caching

!SLIDE

# Caching per model

    @@@ ruby
    class Person
      include Mongoid::Document

      cache
    end

!SLIDE

# Caching per query

    @@@ ruby
    Person.where(:first_name => "Grace").cache

