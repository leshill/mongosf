!SLIDE bullets incremental

# Master / Slave support

* Configuring
* Slave reads

!SLIDE

# Configuring Master / Slave

    @@@ ruby
    defaults: &defaults
      host: localhost
      slaves:
        - host: slave1.local
          port: 27018
        - host: slave2.local
          port: 27019

    development:
      <<: *defaults
      database: development

!SLIDE

# Slave Reads per model

    @@@ ruby
    class Location
      include Mongoid::Document

      enslave
    end

!SLIDE

# Slave Reads per query

    @@@ ruby
    Location.where(:lat_long.near => [30, 30]).enslave

