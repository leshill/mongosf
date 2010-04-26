!SLIDE

# Dirty Attributes

!SLIDE

# Dirty Attributes: Anything changed?

    @@@ ruby
    person = Person.new(:first_name => "Leroy")
    person.first_name = "Lauren"
    person.changed? # =>  true
    person.first_name_changed? # => true

!SLIDE

# Dirty Attributes: Tracking changes

    @@@ ruby
    person.changed? # =>  true
    person.first_name_was # => Leroy
    person.first_name_change #  => [ "Leroy", "Lauren" ]
    person.changes # => { "first_name" => [ "Leroy", "Lauren" ] }

!SLIDE

# Versioning

    @@@ ruby
    class Person
      include Mongoid::Document
      include Mongoid::Versioning
    end

    person.first_name # => Leroy
    person.update_attributes(:first_name => "Lauren")

    person.first_name # => Lauren
    person.versions.last.first_name # => Leroy

!SLIDE

# Timestamps

    @@@ ruby
    class Person
      include Mongoid::Document
      include Mongoid::Timestamps
    end

    person.save # => Leroy
    person.updated_at # Fri Apr 30 19:09:31 UTC 2010

    person.update_attributes(:first_name => "Lauren")
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


