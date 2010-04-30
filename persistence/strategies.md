!SLIDE

# Inserting a new document

    @@@ ruby
    jake = Person.new(:last_name => "Sully")
    jake.save

    # MongoDB query:
    insert([{"created_at"=>Fri Apr 30 15:05:40 UTC 2010,
      "updated_at"=>Fri Apr 30 15:05:40 UTC 2010,
      "_id"=>"4bdaf1c42557ab099f00000b",
      "_type"=>"Person",
      "clearance_level"=>0,
      "last_name"=>"Sully"}])

!SLIDE

# Inserting a tree of data

    @@@ ruby
    jake = Person.new(:last_name => "Sully")
    jake.assignments.build(:name => "Avatar Security")
    jake.save

    # MongoDB query:
    insert([{"created_at"=>Fri Apr 30 15:07:14 UTC 2010,
      "updated_at"=>Fri Apr 30 15:07:14 UTC 2010,
      "_id"=>"4bdaf20f2557ab099f00000c",
      "_type"=>"Person",
      "clearance_level"=>0,
      "last_name"=>"Sully",
      "assignments"=>[{
        "name"=>"Avatar Security",
        "_id"=>"4bdaf2202557ab099f00000d",
        "_type"=>"Assignment"}]}])

!SLIDE

# Embeds one

    @@@ ruby
    jake = Person.create(:last_name => "Sully")
    jake.contact_info = ContactInfo.new(:number => "4348723")
    jake.save

    # MongoDB query
    update({"_id"=>"4bdaf2d82557ab099f00000e"},
      {"$set"=>{
        "contact_info"=>{
          "handle"=>"jsully",
          "_id"=>"4bdaf3142557ab099f00000f",
          "_type"=>"ContactInfo"}}})

!SLIDE

# Embeds many

    @@@ ruby
    jake = Person.create(:last_name => "Sully")
    assignment = Assignment.new(:name => "Avatar Security")
    jake.assignments << assignment
    assignment.save

    # MongoDB query:
    update({"_id"=>"4bdaf48c2557ab099f000014"},
      {"$push"=>{
        "assignments"=>{
          "name"=>"Avatar Security",
          "_id"=>"4bdaf48e2557ab099f000015",
          "_type"=>"Assignment"}}})

!SLIDE

# Updating

    @@@ ruby
    jake = Person.where(:last_name => "Sully").first
    jake.first_name = "Jake"
    jake.save

    # MongoDB query:
    update({"_id"=>"4bdaf48c2557ab099f000014"},
      {"$set"=>{
        "updated_at"=>Fri Apr 30 15:20:20 UTC 2010,
        "first_name"=>"Jake"}})

!SLIDE

# Updating embeds one

    @@@ ruby
    jake = Person.where(:last_name => "Sully").first
    jake.contact_info.handle = "pandora_rox"
    jake.save

    # MongoDB query:
    update({"_id"=>"4bdaf48c2557ab099f000014",
      "contact_info._id"=>"4bdaf5d42557ab099f000016"},
      {"$set"=>{
        "contact_info.handle"=>"pandora_rox"}})

!SLIDE

# Updating embeds many

    @@@ ruby
    jake = Person.where(:last_name => "Sully").first
    assignment = jake.assignments.first
    assignment.name = "Avatar Spy"
    assignment.save

    # MongoDB query:
    update({"_id"=>"4bdaf48c2557ab099f000014",
      "assignments._id"=>"4bdaf48e2557ab099f000015"},
      {"$set"=>{
        "assignments.0.name"=>"Avatar Spy"}})

!SLIDE

# Deleting

    @@@ ruby
    jake = Person.where(:last_name => "Sully").first
    jake.destroy

    # MongoDB query:
    remove({:_id=>"4bd9e4622557ab0676000002"})

!SLIDE

# Deleting embeds one

    @@@ ruby
    jake = Person.where(:last_name => "Sully").first
    jake.contact_info.destroy

    # MongoDB query:
    update({"_id"=>"4bdaf48c2557ab099f000014"},
      {"$unset"=>{
        "contact_info"=>true}})
!SLIDE

# Deleting embeds many

    @@@ ruby
    jake = Person.where(:last_name => "Sully").first
    assignment = jake.assignments.first
    assignment.delete

    # MongoDB query:
    update({"_id"=>"4bdaf48c2557ab099f000014"},
      {"$pull"=>{
        "assignments"=>{
        "_id"=>"4bdaf48e2557ab099f000015"}}})
