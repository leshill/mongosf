!SLIDE

# Mongoid

    @@@ ruby
    class Person
      include Mongoid::Document

      field :first_name
      field :last_name
      field :arrival_date, :type => Date
      field :division
      field :clearance_level, :type => Integer, :default => 0

      embeds_many :assignments

      scope :security_personnel, where(:division => 'security')
    end

    jake = Person.security_personnel.where(:first_name => 'Jake',
                                           :last_name => 'Sully').first

!SLIDE

# Mongoid::Document

    @@@ ruby
    class Person
      include Mongoid::Document

      field :first_name
      field :last_name
      field :arrival_date, :type => Date
      field :division
      field :clearance_level, :type => Integer, :default => 0
    end

!SLIDE

# Associations

    @@@ ruby
    class Person
      include Mongoid::Document

      embeds_many :assignments
    end

    class Assignment
      include Mongoid::Document

      embedded_in :person, :inverse_of => :assignments
    end

!SLIDE

# Validations

    @@@ ruby
    class Assignment
      include Mongoid::Document

      field :name
      field :start_date, :type => Date
      field :end_date, :type => Date

      validates_presence_of :name, :start_date
    end

!SLIDE

# Finders

    @@@ ruby
    Person.all(:conditions => {:first_name => "Grace"})

    Person.first

    Person.find_or_create_by(:first_name => "Miles")

!SLIDE

# Callbacks

    @@@ ruby
    class Person
      include Mongoid::Document

      field :first_name
      field :last_name

      before_save :update_current_location
    end

!SLIDE

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

