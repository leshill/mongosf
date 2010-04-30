!SLIDE bullets incremental

# Criteria

* Queries
* Sorting and Pagination
* Limiting returned fields
* Geo support (RENAME this)
* Scopes

!SLIDE

# Find by id

    @@@ ruby
    Person.criteria.id("4bd9f19f2557ab0676000003").first

    # MongoDB query:
    find({:_id=>"4bd9f19f2557ab0676000003"}, {}).limit(-1)

!SLIDE

# Where

    @@@ ruby
    Person.where(:clearance_level.gt => 3).entries

    # MongoDB query:
    find({:clearance_level=>{"$gt"=>3}}, {})

!SLIDE

# `$all`

    @@@ ruby
    Person.all_in(:first_name => ['Grace', 'Jake']).entries

    # MongoDB query:
    find({:first_name=>{"$all"=>["Grace", "Jake"]}}, {})

!SLIDE

# `$in`

    @@@ ruby
    Person.any_in(:first_name => ['Grace', 'Jake']).entries

    # MongoDB query:
    find({:first_name=>{"$in"=>["Grace", "Jake"]}}, {})

!SLIDE

# `$ne`

    @@@ ruby
    Person.excludes(:first_name => 'Grace').entries

    # MongoDB query:
    find({:first_name=>{"$ne"=>"Grace"}}, {})

!SLIDE

# `$nin`

    @@@ ruby
    Person.not_in(:first_name => 'Jake').entries

    # MongoDB query:
    find({:first_name=>{"$nin"=>["Jake"]}}, {})

!SLIDE

# `$where`

TODO

!SLIDE

# Sorting

    @@@ ruby
    Person.order_by([:last_name, :asc]).entries

    # MongoDB query:
    find({}, {}).sort([:last_name, :asc])

!SLIDE

# Pagination

    @@@ ruby
    Person.skip(10).limit(10).entries

    # MongoDB query:
    find({}, {}).skip(10).limit(10)

!SLIDE

# Limiting returned fields

    @@@ ruby
    Person.only(:first_name, :last_name).entries

    # MongoDB query:
    find({}, {:_type=>1, :last_name=>1, :first_name=>1})

!SLIDE

# Geo support

    @@@ ruby
    class Location
      include Mongoid::Document
      field :name
      field :lat_long, :type => Array
      index [[:lat_long, Mongo::GEO2D]]
    end

    Location.near(:lat_long => [30, 30])

    # MongoDB query:
    find({:lat_long=>{"$near"=>[30, 30]}}, {})

