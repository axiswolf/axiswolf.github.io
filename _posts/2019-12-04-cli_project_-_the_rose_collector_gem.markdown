---
layout: post
title:      "CLI Project - The Rose Collector Gem"
date:       2019-12-04 06:26:34 +0000
permalink:  cli_project_-_the_rose_collector_gem
---

I've been getting into gardening lately to de-stress myself and get out more. I started to look into a bunch of websites that sell bushes and was amazed by all of the types of roses. That's where my idea for this gem came from.
I wanted to do something more conventional like... recipes, but I feel like it's been done many times before.

I probably watched Avi's video about 20 times over the last couple of months. There's something about these projects that scare me but I have to do it. No more procastination.

Basically, I started off by hard-coding my gem like he suggests to get things rolling.

My directory looks like this for now:
* bin 
* config
* lib
* Gemfile
* Gemfile.lock
* Rakefile
* README.md
* rosecollector.gemspec

Step 1 in `#bin/console`, I require the environment in which we load the needed gems and the lib files.
```
require_relative '../config/environment'
require './lib/rosecollector

Rosecollector::CLI.new.call
```

`Rosecollector::CLI.new.call ` hasn't been made yet and should reside in the lib folder.

--> touch lib/rosecollector/cli.rb
```
class Rosecollector::CLI

def call
puts "Roses Roses Roses"
get_roses #create method to list roses
menu #create method that interacts with user
good_bye #create method that bids user farewell
end 

end
```

From here, we create methods within the` Rosecollector::CLI ` to interact with the user via terminal. 

Now to move away from the "fake data" and hard-coding, we make another class within the lib folder. 
--> touch lib/rosecollector/roses.rb

```
class Rosecollector::Roses
    attr_accessor :name, :price, :url
    def self.all
        #  need to scrape these 
        Rosecollector::Scraper.scrape

        rose_1 = self.new
        rose_1.name = "rose 1 name"
        rose_1.price = "$27"
        rose_1.url = "url"
        
        rose_2 = self.new
        rose_2.name = "rose 2 name"
        rose_2.price = "$27"
        rose_2.url = "url"
        
        [rose_1, rose_2]
    end

end

```
Going back to the ` Rosecollector::CLI `, we fix the method `#get_roses`
```
    def get_roses
        @roses = Rosecollector::Roses.all
        @roses.each.with_index(1) do |rose, i|
            puts "#{i}. #{rose.name} #{rose.price}"
        end
    end
```

So, I head back to my #roses.rb file and require the right gems to start scraping-- Nokogiri and Open-URI

```
require 'nokogiri'
require 'open-uri'
require 'pry'
```

I start to write my scraper 
```
class Rosecollector::Roses
    attr_accessor :name, :price, :url, :description

    def self.scrape_site
        doc = Nokogiri::HTML(open("https://www.edmundsroses.com/category/8"))
        @@roses = []
        doc.css("section.product").each do |r|
            rose = self.new
            rose.name = r.css("h2").text.strip
            rose.price = r.css(".price").text.strip
            rose.url = "https://www.edmundsroses.com" + r.css(".image a").attr("href")
                doc2 = Nokogiri::HTML(open(rose.url))
                rose.description = doc2.css("div.prod_desc").text.strip
            @@roses << rose
        end
        @@roses
    end

end

```

