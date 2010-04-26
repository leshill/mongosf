!SLIDE incremental bullets

# Basics

* document
* associations
* validations
* persistence
* finders
* callbacks

!SLIDE ruby

# Mongoid::Document

    @@@ ruby
    class Person
      include Mongoid::Document
      field :first_name
      field :last_name
      field :arrival_date, :type => Date
      field :clearance_level, :default => 'basic'
    end

!SLIDE small

# Associations

    @@@ ruby
    class Person
      include Mongoid::Document
      embeds_many :locations
    end

    class Location
      include Mongoid::Document
      field :lat, :type => Float
      field :long, :type => Float
      embedded_in :locatable, :inverse_of => :locations
    end

!SLIDE small

# Validations

    @@@ ruby
    class Location
      include Mongoid::Document
      field :lat, :type => Float
      field :long, :type => Float

      validates_presence_of :lat, :long
    end

!SLIDE smaller

# Persistence

    @@@ ruby
    grace = Person.new(:first_name => "Grace",
                       :last_name => "Augustine")
    grace.save

    jake = Person.create(:first_name => "Jake",
                         :last_name => "Sully")
    jake.update_attributes(:first_name => "Toruk",
                           :last_name => "Makto")

    miles = Person.create(:first_name => "Miles",
                          :last_name => "Quaritch")
    miles.destroy

!SLIDE small

# Finders

    @@@ ruby
    Person.all(:conditions => {:first_name => "Grace"})

    Person.first

    Person.find_or_create_by(:first_name => "Miles")

!SLIDE small

# Callbacks

    @@@ ruby
    class Person
      include Mongoid::Document

      field :first_name
      field :last_name

      before_save :update_current_location
    end

