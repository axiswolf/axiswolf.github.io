---
layout: post
title:      "CLI Project - The Rose Collector Gem"
date:       2019-12-04 01:26:35 -0500
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

To give you a better idea of my dependencies, I made a little flow chart:
![](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=Roses%20CLI%20Dependencies.drawio#R7ZlPU6MwGMY%2FDcd1SAL9c1xbdQ%2FrjjM9qMcIsWQ2Jd2QWrqffoMkFAgOtEqpzs54IC9JSH7vw%2FMG66DZKr0ReB3d8pAwB7ph6qC5AyHwIHSyPzfc5ZExQHlgKWioO%2B0DC%2FqX6KCroxsakqTSUXLOJF1XgwGPYxLISgwLwbfVbs%2BcVZ%2B6xktiBRYBZnb0noYyyqMT393HfxC6jMyTgavvrLDprANJhEO%2BLYXQlYNmgnOZX63SGWEZPMMlH3f9xt1iYYLEsssA%2FGtxg1Y%2Ft%2BA2mv65T%2B8efnvom87OC2YbveEnGqs5FM6EKwb5yuXO4NhGVJLFGgdZe6tS7qDLSK6YagF1iZN1noRnmhL13Es9PRGSpG%2BuGxQ0lIwIXxEpdqqLHoAmGqBWEPR0e7vPx1iHolIqTAxrBSyLmfeQ1IXmdAAzZDPbxCEjQs2WELlZW9QEVx0yHnNXMWlh%2BAHIfFhFBqCNrBBqmZnXFzPPYiZ4QgLOmNILF%2BdHbNyRGOqLmG8Ro%2BLJ4qQmUk5IBmHUgKjpPexNU8A2L5tPHH7PqoBqBQwnCQ2qWNTWxe5By%2By18Vi%2BM0%2FLt%2BY700qpfDD91PVjKb4fkjXMiHxhJLSKTQ2%2BWjzfiIC0W7bEYklk2ytnJ7OULb8hWyYmCMOSvlSX25RC%2FYQ7TtVGCq14bs20pzUV5NvUo8pVqz6R%2F4b7m4lyDtZEr4oqtv0OkdnOlVfIrv4FTvJuIt%2BvYEJNjg8b8j3q7e20DUwdKp7pUs1G4hcq1HN0yoY0%2FlHN1BDoCK434wejBlu7ZvTpANGdht20AzpwUnTjgypCzOOscoY4iYpXtbU2uBd%2BS3Xo7vWtHg4GNXFYM3FUT1xXE4d1E5%2Bc2MQnX00X%2FqDFfVwrNv6RuvBhbSL3xLqYWrq4sF32WplkQnk8uN%2FWPoSLD%2BOy36JTnsBNXWzhFzB6buy8hn8iNLLrrVaZLbSwy9rJ2dGbDk7PPpa%2Fw9G7OvNQhjuq4vfBkX47aqnnPdsttD8JPnkZHp2RKpDBe6gqijP6QKcz2PTB86llMT0jWXj1M9WxsrBOeX3L4qsd2ku%2F%2BpyBLPwPcguvXox6lgWyz03%2FK39r1iYt8xydtNcjqvkJM%2B%2B%2B%2FyEYXf0D)

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
        @roses = []
        doc.css("section.product").each do |r|
            rose = self.new
            rose.name = r.css("h2").text.strip
            rose.price = r.css(".price").text.strip
            rose.url = "https://www.edmundsroses.com" + r.css(".image a").attr("href")
                doc2 = Nokogiri::HTML(open(rose.url))
                rose.description = doc2.css("div.prod_desc").text.strip
            @roses << rose
        end
        @roses
    end

end

```


Lastly, I write my menu option in the cli.rb file.



```
def menu
        puts "Enter the number associated with rose:"
        puts "type 'list' for rose list or 'exit'"
        input = nil
        while input != "exit"
            input = gets.strip.downcase
            if input.to_i > 0 && input.to_i <= @roses.count
                rose = @roses[input.to_i-1]
                puts "#{rose.name}"
                puts "#{rose.url}"
                puts "#{rose.price}"
                puts "#{rose.description}"
                puts "Type 'list' to view the list again, or 'exit'"
            elsif input =="list"
                get_roses
            elsif input.to_i > @roses.count
                puts "We don't have that, type 'list' or 'exit'"
            end 
        end
        goodbye  
    end
```

