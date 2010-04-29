!SLIDE small

# Persistence (new document)

    @@@ ruby
    # In Mongoid:
    Person.create(:last_name => "Dawkins")

    richard = Person.new(:last_name => "Dawkins")
    richard.save

    # MongoDB query:
    db.people.insert({ "last_name" : "Dawkins" }, true);

!SLIDE small

# Persistence (new tree)

    @@@ ruby
    # In Mongoid:
    richard = Person.new(:last_name => "Dawkins")
    richard.addresses.build(:street => "Upper St")
    richard.save

    # MongoDB query:
    db.people.insert(
      {
        "last_name" : "Dawkins",
        "addresses" : [ { "street" : "Upper St" } ]
      },
      true
    );

!SLIDE small

# Persistence (new embeds_one)

    @@@ ruby
    # In Mongoid:
    richard = Person.create(:last_name => "Dawkins")
    email = Email.new(:address => "rd@dawkins.net")
    email.save

    # MongoDB query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { $set :
        { "email" :
          { "address" : "rd@dawkins.net" }
        }
      },
      false,
      true
    );

!SLIDE small

# Persistence (new embeds_many)

    @@@ ruby
    # In Mongoid:
    richard = Person.create(:last_name => "Dawkins")
    address = Address.new(:street => "Upper St.")
    richard.addresses << address
    address.save

    # MongoDB query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { $push :
        { "addresses" :
          { "street" : "Upper St." }
        }
      },
      false,
      true
    );

!SLIDE small

# Persistence (update document)

    @@@ ruby
    # In Mongoid:
    richard = Person.where(:last_name => "Dawkins").first
    richard.first_name = "Richard"
    richard.save

    # MongoDB query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { $set :
        { "first_name" : "Richard" }
      },
      false,
      true
    );

!SLIDE small

# Persistence (update embeds_one)

    @@@ ruby
    # In Mongoid:
    richard = Person.where(:last_name => "Dawkins").first
    richard.email.address = "rick@dawkins.net"
    richard.save

    # MongoDB query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { $set :
        { "email.address" : "rick@dawkins.net" }
      },
      false,
      true
    );

!SLIDE small

# Persistence (update embeds_many)

    @@@ ruby
    # In Mongoid:
    richard = Person.where(:last_name => "Dawkins").first
    address = richard.addresses.first
    address.street = "Clarkenwell Rd"
    address.save

    # MongoDB query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { $set :
        { "addresses.0.street" : "Clarkenwell Rd" }
      },
      false,
      true
    );

!SLIDE small

# Persistence (delete document)

    @@@ ruby
    # In Mongoid:
    richard = Person.where(:last_name => "Dawkins").first
    richard.delete # or destroy

    # MongoDB query:
    db.people.remove(
      { "_id" : "4baa56f1230048567300485c" },
      true
    );

!SLIDE small

# Persistence (delete embeds_one)

    @@@ ruby
    # In Mongoid:
    richard = Person.where(:last_name => "Dawkins").first
    richard.email.delete

    # MongoDB query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { $unset :
        { "email" : true }
      },
      true
    );

!SLIDE small

# Persistence (delete embeds_many)

    @@@ ruby
    # In Mongoid:
    richard = Person.where(:last_name => "Dawkins").first
    address = richard.addresses.first
    address.delete

    # MongoDB query:
    db.people.update(
      { "_id" : "4baa56f1230048567300485c" },
      { $pull :
        { "addresses" :
          { "_id" : "4baa56f1230048567300485d" }
        }
      },
      true
    );
