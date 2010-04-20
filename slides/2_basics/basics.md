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
      field :middle_initial
      field :last_name
      field :birthday, :type => Date
      field :blood_alcohol_level, :type => Float, :default => 0.0
    end

!SLIDE small

# Associations

    @@@ ruby
    class Person
      include Mongoid::Document
      field :first_name
      field :last_name
      embeds_one :address
      embeds_many :phones
    end

    class Phone
      include Mongoid::Document
      field :country_code, :type => Integer, :default => 1
      field :number
      embedded_in :person, :inverse_of => :phones
    end

!SLIDE small

# Validations

    @@@ ruby
    class Phone
      include Mongoid::Document
      field :country_code, :type => Integer, :default => 1
      field :number

      validates_format_of :number, :with => /[\d\(\)\-]*/
    end

!SLIDE small

# Persistence

    @@@ ruby
    darth = Person.create(:first_name => "Anakin", :last_name => "Skywalker")
    darth.update_attributes(:first_name => "Darth", :last_name => "Vader")

    person.destroy
