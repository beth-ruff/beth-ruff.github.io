---
layout: post
title:      "All Hail ActiveRecord!"
date:       2020-04-29 23:06:14 +0000
permalink:  all_hail_activerecord
---


I have to say, ActiveRecord is my favorite Ruby Gem I've learned about thus far. It truly seems like magic is happening under the hood, which is why I understand  the need to learn about ORMs without ActiveRecord first. But I'm getting ahead of myself...let's start at the beginning.

Object Relational Mapping allows you to access a relational database using OO Ruby (or any object-oriented programming language). You can map a Class to a table, map tables to Ruby objects, and build methods to update database records using Ruby objects. On top of this, there is dynamic object relational mapping that abstracts our code and allows our methods to be used in multiple classes. A good example of dynamic ORM work is the Learn.co Dynamic ORM Lab. Here is a snippet of my code from one of my classes below:

```
class InteractiveRecord
  
  def self.table_name
    self.to_s.downcase.pluralize
  end 
  
  def self.column_names
    sql = "PRAGMA table_info('#{table_name}')"
    
    table_info = DB[:conn].execute(sql)
    column_names = []
    
    table_info.each do |column|
      column_names << column["name"]
    end 
    column_names.compact
  end 
  
  def initialize(options={})
    options.each do |property, value|
      self.send("#{property}=", value)
    end 
  end 
  
  def table_name_for_insert
    self.class.table_name 
  end 
  
  def col_names_for_insert
    self.class.column_names.delete_if {|col| col == "id"}.join(", ")
  end 
  
  def values_for_insert
    values = []
    self.class.column_names.each do |col_name|
      values << "'#{send(col_name)}'" unless send(col_name).nil?
    end 
    values.join(", ")
  end 
  
  def save
    sql = "INSERT INTO #{table_name_for_insert} (#{col_names_for_insert}) VALUES (#{values_for_insert})"
    DB[:conn].execute(sql)
    @id = DB[:conn].execute("SELECT last_insert_rowid() FROM #{table_name_for_insert}")[0][0]
  end 
  

    DB[:conn].execute(sql, value_name);
  end 
  
end
```

Here you can see I've built attr_accessors from column names, abstracted the initialize method, built ORM methods to insert column names and values, and finally saved these values to the database. 

This code is all fine and dandy, but while working through the labs, I realized I was trying to memorize the dynamic methods (and having trouble doing so) since some of them are rather complicated. This was frustrating to me to the point where I made flash cards to familiarize myself with each of the methods.

But then, ActiveRecord magically came into my life! I'm not kidding when I say I had the biggest smile on my face as I went through these ActiveRecord lessons. AR is a Ruby gem that has built-in ORM methods using class inheritance. Some of these methods include .column_names, .create, .find, .find_by, #attr_accessors, and #save. AKA everything I had been trying and failing to memorize the last couple of weeks. The only code that has to be written with AR is this:

```
class Dog < ActiveRecord::Base
end
```

How beautiful!!

![](https://giphy.com/embed/YnBntKOgnUSBkV7bQH)

In the end, I'm thankful for the struggle of learning dynamic ORM methods so that I can truly appreciate everything ActiveRecord is doing. Although I'm even more thankful AR exists!
