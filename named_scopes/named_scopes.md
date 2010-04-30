!SLIDE bullets incremental

# Scopes

    @@@ ruby
    class Person
      scope :security_personnel, where(:division => 'security')
    end

    Person.security_personnel.entries

    # MongoDB query:
    find({:division=>"security"}, {})

!SLIDE

# Scopes accept parameters

    @@@ ruby
    class Person
      scope :cleared_for, lambda { |level|
        where(:clearance_level.gte => level)
      }
    end

    Person.cleared_for(2).entries

    # MongoDB query:
    find({:clearance_level=>{"$gte"=>2}}, {})

!SLIDE

# Chain with scopes or criteria

    @@@ ruby
    Person.security_personnel.cleared_for(1).entries

    # MongoDB query:
    find({:clearance_level=>{"$gte"=>1}, :division=>"security"}, {})

    Person.security_personnel.where(:first_name => 'Jake').entries

    # MongoDB query:
    find({:division=>"security", :first_name=>"Jake"}, {})
