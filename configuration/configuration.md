!SLIDE

# Configuration

!SLIDE

# Master / Slave support

!SLIDE

# Master / Slave support

## Read from Slaves per model

    @@@ ruby
    class Location
      include Mongoid::Document
      enslave
    end

# Master / Slave support

## Read from Slaves per query

    @@@ ruby
    Location.where(:lat_long.near => [30, 30]).enslave

