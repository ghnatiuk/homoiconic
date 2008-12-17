Another program to compute the nth Fibonacci number
===

It has been few days since I wrote [A program to compute the nth Fibonacci number](http://github.com/raganwald/homoiconic/tree/master/2008-12-12/fibonacci.md). Some great job leads have come in, although at this moment [I'm still available](http://reginald.braythwayt.com/RegBraithwaiteGH1208_en_US.pdf "Reginald Braithwaite's Resume"). To those of you who got in touch or passed my name along, thank you. And while I have a ton of stuff on my plate, "All writing is rewriting." So I had another look the Fibonacci program, and I rewrote it.

> Now this is not an advocacy blog, so I will not launch into an essay. But if I can share one little opinion... I think we all agree with Abelson and Sussman that we write programs primarily for humans to read, and secondarily for compilers to execute. Our experience with communicating with people is that we get the best results when we design several alternate approaches and compare them to each other. Designing programs (whether code in the small or architecture in the large) is no different.

> If an Architect is given a problem and comes back with a single design encompassing the "Best Practices," I am immediately suspicious. If the same architect comes back with three approaches and can articulate the advantages and disadvantages of each approach given the specific problem being solved, I am always impressed. A good designer is always able to come up with several good approaches. One will be chosen, but rarely if ever are there no decent alternatives.

> And thus, I consider solving the same problem several different ways to be a useful exercise. Possibly not as immediately rewarding as returning calls from recruiters trying to staff J2EE positions at BigCo, but useful nonetheless.

The second approach still uses the Matrix algorithm for calculating the _nth_ number in the Fibonacci sequence, however there are a number of key differences from the first version:

	module Fibonacci
  
	  Matrix = Struct.new(:a, :b, :c) do
    
	    alias :d :a
	    alias :e :b
	    alias :f :c
    
	    def * other
	      Matrix.new(
	        self.a * other.d + self.b * other.e, 
	        self.a * other.e + self.b * other.f,
	        self.b * other.e + self.c * other.f
	      )
	    end
    
	    def ^ n
	      if n == 1
	        self
	      elsif n == 2
	        self * self
	      elsif n > 2
	        if n % 2 == 0
	          self ^ (n / 2) ^ 2
	        else
	          (self ^ (n / 2) ^ 2) * self
	        end
	      end
	    end
    
	  end
  
	  def self.[] n
	    return n if n < 2
	    (Matrix.new(1,1,0) ^ (n - 1)).a
	  end
  
	end

In this version, `Fibonacci` is an entity in Kernel namespace. You do not call `n.matrix_fib`, you ask for `Fibonacci[n]`. The trade-off there is between naive object-orientation ("Integers should know their own fibonacci watchamacallits") and having a first-class entity. The naive OO interpretation is frankly suspect. If it makes sense for the integer `14` to be responsible for knowing that the fourteenth number of the Fibonacci sequence is `377`, why doesn't it make sense for the integer `14` to also be responsible for knowing which Customer Record has `id = 14`? Why don't we write `14.customer` the ways Rails people write `3.days.ago`?

Also, the class `Fibonacci::Matrix` explicitly defines `*` and `^` so that we can write arithmetic operations on matrices the way we write them on integers. This is one of the prime motivations for languages like Ruby to permit operator overloading. `Fibonacci::Matrix` takes advantage of `Struct`. You can read [all about Struct](http://blog.grayproductions.net/articles/all_about_struct) if you are not familiar with this handy tool. Note that `Struct.new` returns a class, not an instance of a Struct. This is a very handy paradigm for Ruby programs.

Share and enjoy!

----
	
Subscribe to [new posts and daily links](http://feeds.feedburner.com/raganwald "raganwald's rss feed"): <a href="http://feeds.feedburner.com/raganwald"><img src="http://feeds.feedburner.com/~fc/raganwald?bg=&amp;fg=&amp;anim=" height="26" width="88" style="border:0" alt="" align="top"/></a>

My personal home page: [http://reginald.braythwayt.com](http://reginald.braythwayt.com "Reginald Braithwaite")