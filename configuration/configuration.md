!SLIDE bullets incremental

# Configuration

* Dynamic Fields
* Using ObjectIds
* Persisting type information
* Connections
* Configuring options

!SLIDE

# Dynamic Fields

    @@@ ruby
    class Location
      include Mongoid::Document

      field :lat_long, :type => Array
    end

    loc = Location.new(:name => "Tree of Voices")
    loc.name # => "Tree of Voices"

!SLIDE

# Use Object Ids

    @@@ ruby
    person = Person.create(:first_name => "Trudy", :last_name => "Chacon")

    # use_objects_ids = true
    person.id # => ObjectID('4bd648a12557abeebd000002')

    # use_objects_ids = false
    person.id # => "4bd64b942557abef00000002"

!SLIDE

# Persisting type information

    @@@ ruby
    person = Person.create(:first_name => "Trudy", :last_name => "Chacon")

    # persist_types = true
    person._type # => "Person"

    # persist_types = false
    person._type # => nil

!SLIDE bullets

# Connections

* Works with a default install and startup
* `localhost:27017`

!SLIDE

# Connections

## `config/mongoid.yml`

    @@@ ruby
    defaults: &defaults
      host: localhost

    development:
      <<: *defaults
      database: development

!SLIDE

# Configuring options

## `config/mongoid.yml`

    @@@ ruby
    defaults: &defaults
      allow_dynamic_fields: true
      use_object_ids: true

    development:
      <<: *defaults

!SLIDE

# Configuring options

## `config/initializer/mongoid.rb`

    @@@ ruby
    Mongoid.configure do |config|
      config.use_object_ids = false
      config.persist_types = true
    end

!SLIDE

# Configuring the logger

## `config/initializer/mongoid.rb`

    @@@ ruby
    Mongoid.configure do |config|
      old_connection = config.master.connection
      old_db = config.master
      config.master = Mongo::Connection.new(old_connection.host,
        old_connection.port,
        :logger => Rails.logger).db(old_db.name)
    end
