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
