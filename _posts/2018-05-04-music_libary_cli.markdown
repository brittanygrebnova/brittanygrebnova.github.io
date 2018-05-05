---
layout: post
title:      "Music Libary CLI"
date:       2018-05-05 03:07:06 +0000
permalink:  music_libary_cli
---


It came. It saw. It conquered my entire life for a whole week.

The Music Library CLI was the first of the final projects for Object Oriented Ruby. To say that it was challenging is an understatement; it truly tested me on all the knowledge I had gained in the preceding lessons. In this blog post, I would like to iterate over ; ) the final method that brought everything together - #play_song.

The first line:

  def play_song
    puts "Which song number would you like to play?"

The CLI asks the user which song they would like to play from the imported library, a simple puts statement. The CLI then 'gets' the user's response in the next line:

  user = gets.strip
	
Then the program checks to make sure that the number entered by the user is a valid number corresponding to one of the song options in the music library. This is accomplished by the following code:

  if user.to_i.between?(1, Song.all.length)
	
Song.all.length is the number of items in the Song.all array. If the number entered is valid, the CLI then matches the number entered with the corresponding item in the Song.all array through this line of code:

  song = Song.sorted[user.to_i - 1]
	
The song variable is assigned to the value of the sorted class method, which looks like this:

  def sorted
    self.all.uniq.sort_by {|s| s.name}
		  end
			
The return value of this method is a uniqe array of items listed alphabetically. In the case of Song.sorted, it is an array of songs listed in alphabetical order. So calling Song.sorted[user.to_i - 1] takes the user's input, subtracts 1 from the number in order to correspond to the array items that start with a 0 index, and chooses the song at that index from the Song.all array. The next line:

  puts "Playing #{song.name} by #{song.artist.name}"
	
Here, the program uses string interpolation to return the .name and .artist.name of the matching song item in the Song.all array. 

It was interesting to incorporate the use of modules in this lab, which was a concept recently introduced in the course. I really like the sleekness of having a single file with a body of methods that can be applied to each class. 

Here's the final #play_song method:

    def play_song
      puts "Which song number would you like to play?"
      user = gets.strip
        if user.to_i.between?(1, Song.all.length)
          song = Song.sorted[user.to_i - 1]
          puts "Playing #{song.name} by #{song.artist.name}"
        end
    end
		
		Thank you for reading!
